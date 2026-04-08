---
date: 20-03-2026
tags:
status: editing
sticker: lucide//file
---

| Под темы                   | Заметка                                                                                                                                                                                                        |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Требования к сущностям     | Класс сущности должен иметь конструктор без параметров. Класс сущности также может иметь другие конструкторы. Конструктор без параметров должен быть public или protected.                                     |
|                            | Класс сущности не должен быть final. Никакие методы и атрибуты сущности не могут быть final.                                                                                                                   |
|                            | Lazy ассоциации должны загружаться только по явному запросу. В противном случае мы можем столкнуться с проблемами с производительностью или с LazyInitializationException.                                     |
|                            | Реализации `equals()` и `hashCode()` должны учитывать мутабельность сущностей.                                                                                                                                 |
| Конструктор без параметров | Вручную определить конструктор без параметров для всех сущностей в проекте.                                                                                                                                    |
|                            | Использовать плагин для компилятора kotlin-jpa. Этот плагин гарантирует, что конструктор без параметров будет сгенерирован в байт-коде для всех JPA-классов: `@Entity`, `@MappedSuperclass` или `@Embeddable`. |

%%Maven%%
```

<plugin\>   
	<groupId>org.jetbrains.kotlin</groupId>   
	<artifactId>kotlin-maven-plugin</artifactId>   
	<configuration>       
		<compilerPlugins>           
			...           
			<plugin>jpa\</plugin>           
			...       
		</compilerPlugins>   
	</configuration>   
	<dependencies\>       
		...       
		<dependency>           
			<groupId>org.jetbrains.kotlin</groupId>          
			<artifactId>kotlin-maven-noarg</artifactId>           
			<version>${kotlin.version}</version>       
		</dependency>       
		...   
	</dependencies>
</plugin>
```

%%Gradle%%
```
plugins {
  id "org.jetbrains.kotlin.plugin.jpa" version "1.5.21"
}
```

| Под темы               | Заметка                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Open классы и свойства | Согласно спецификации JPA, все классы и свойства, связанные с JPA, не должны быть final. Некоторые реализации JPA не требуют исполнения этого правила. Например, Hibernate не бросает исключение, когда встречает final класс сущности. Однако, от final класса нельзя наследоваться, из-за чего отключается механизм проксирования Hibernate. Нет прокси — нет lazy загрузки. Это означает, что ассоциации ToOne всегда будут не lazy, а eager, что может заметно сказаться на производительности. |
|                        | В отличие от Джавы, в Котлине классы, свойства и методы по умолчанию final. Поэтому их нужно явно помечать ключевым словом `open`.                                                                                                                                                                                                                                                                                                                                                                  |

```
Table(name = "project")
@Entity
open class Project {    
	@Id    
	@GeneratedValue(strategy = GenerationType.IDENTITY)    
	@Column(name = "id", nullable = false)    
	open var id: Long? = null    
	
	@Column(name = "name", nullable = false)    
	open var name: String? = null    
	
	...
}
```

|                        |                                                                                                                                                                                                                                                                                                                  |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Open классы и свойства | Также эту задачу можно решить автоматически с помощью плагина для компилятора all-open. Он помечает нужные классы и свойства ключевым словом `open` прямо в байт-коде. Главное его правильно настроить, чтобы он применялся к классам, помеченным не только `@Entity`, но и `@MappedSuperclass` и `@Embeddable`. |

%%Maven%%
```
<plugin>
   <groupId>org.jetbrains.kotlin</groupId>
   <artifactId>kotlin-maven-plugin</artifactId>   
   <configuration>       
		<compilerPlugins>           
		   ...           
		   <plugin>all-open</plugin>       
		</compilerPlugins>       
		<pluginOptions>           
			<option>all-open:annotation=javax.persistence.Entity</option>          
			<option>all-open:annotation=javax.persistence.MappedSuperclass</option>   
			<option>all-open:annotation=javax.persistence.Embeddable</option>       
		</pluginOptions>   
	</configuration>   
	<dependencies>       
		<dependency>           
			<groupId>org.jetbrains.kotlin</groupId>           
			<artifactId>kotlin-maven-allopen</artifactId>           
			<version>${kotlin.version}</version>       
		</dependency>   
	</dependencies>
</plugin>
```

%%Gradle%%
```
plugins {
  id "org.jetbrains.kotlin.plugin.allopen" version "1.5.21"
}

allOpen {
    annotations("javax.persistence.Entity", "javax.persistence.MappedSuperclass", "javax.persistence.Embedabble")
}
```

|                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Непреднамеренная загрузка lazy ассоциаций | Ассоциации ToMany по умолчанию lazy, и это не просто так: их загрузка без необходимости может серьезно сказаться на производительности. Реализации `equals()`, `hashCode()` и `toString()` в классах данных используют все поля из первичного конструктора, куда могут входить и lazy ассоциации. В таком случае, вызов этих методов может привести к нежелательным запросам к БД или исключению `LazyInitializationException`. |

```
@Override
override fun toString(): String {   
	return this::class.simpleName + "(id = $id , name = $name )"
}
```

|                                  |                                                                                                                                                                                                                                           |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Проблема с equals() и hashCode() | Сущности мутабельны по своей природе. Даже ID зачастую генерируется базой данных, то есть изменяется после первого сохранения сущности. Получается, не существует полей, на основе которых можно было бы консистентно вычислить hashCode. |

```
override fun equals(other: Any?): Boolean {   
	if (this === other) return true   
	if (other == null || Hibernate.getClass(this) != Hibernate.getClass(other)) 
		return false   
		
	other as Client   
	
	return id != null && id == other.id
}

override fun hashCode(): Int = 1756406093
```


|                   |                                                                                                                                                                                                                         |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Null безопасность | Одно из преимуществ Котлина перед Джавой — встроенная null безопасность (null safety). Она также может быть обеспечена на стороне БД с помощью ограничений NOT NULL. Эти два подхода можно и нужно использовать вместе. |

```
@Table(name = "contact")
@Entity
class Contact(   
	@NaturalId   
	@Column(name = "email", nullable = false, updatable = false)   
	val email: String,   
	
	@Column(name = "name", nullable = false)   
	var name: String   
	
	@ManyToOne(fetch = FetchType.LAZY, optional = false)   
	@JoinColumn(name = "client_id", nullable = false)   
	var client: Client) {   
		// id and other properties omitted
	}
```


> [!NOTE] Замечание
> Если нужно исключить поле из конструктора
>

```
@Entitydata 
class Contact(   
	@NaturalId   
	@Column(name = "email", nullable = false, updatable = false)   
	val email: String,) {   
		@Column(name = "name", nullable = false)   
		var name: String = ""   
		
		@ManyToOne(fetch = FetchType.LAZY, optional = false)   
		@JoinColumn(name = "client_id", nullable = false)   
		lateinit var client: Client   
		
		// id and other properties omitted
	}
```

