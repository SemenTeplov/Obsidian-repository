---
date: 23-03-2026
tags:
  - IT
  - Development
  - JAVA
  - Spring
status: ready
sticker: lucide//file-check
---

| Под темы    | Заметка                                                              |
| ----------- | -------------------------------------------------------------------- |
| Определение | @Qualifier - аннотация, которая позволяет выбирать бин для инъекции. |

#### Для описания

%%Java%%
```
public class MovieRecommender { 
	@Autowired 
	@Qualifier("main") 
	private MovieCatalog movieCatalog; 
	
	// ... 
}
```

%%Kotlin%%
```
class MovieRecommender { 
	@Autowired 
	@Qualifier("main") 
	private lateinit var movieCatalog: MovieCatalog 
	
	// ... 
}
```

#### Аннотирование отдельных параметров

%%Java%%
```
public class MovieRecommender { 
	private final MovieCatalog movieCatalog; 
	
	private final CustomerPreferenceDao customerPreferenceDao; 
	
	@Autowired 
	public void prepare(@Qualifier("main") MovieCatalog movieCatalog, CustomerPreferenceDao customerPreferenceDao) { 
		this.movieCatalog = movieCatalog; this.customerPreferenceDao = customerPreferenceDao; 
	} 
	
	// ... 
}
```

%%Kotlin%%
```
class MovieRecommender { 
	private lateinit var movieCatalog: MovieCatalog 
	private lateinit var customerPreferenceDao: CustomerPreferenceDao 
	
	@Autowired 
	fun prepare(@Qualifier("main") movieCatalog: MovieCatalog, customerPreferenceDao: CustomerPreferenceDao) { 
		this.movieCatalog = movieCatalog 
		this.customerPreferenceDao = customerPreferenceDao 
	} 
		
	// ... 
}
```

#### Создание собственного квалификатора

Создание:

%%Java%%
```
@Target({ElementType.FIELD, ElementType.PARAMETER}) 
@Retention(RetentionPolicy.RUNTIME) 
@Qualifier 
public @interface Genre { 
	String value(); 
}
```

%%Kotlin%%
```
@Target(AnnotationTarget.FIELD, AnnotationTarget.VALUE_PARAMETER) 
@Retention(AnnotationRetention.RUNTIME) 
@Qualifier 
annotation class Genre(val value: String)
```

Применение:

%%Java%%
```
public class MovieRecommender { 

	@Autowired 
	@Genre("Action") 
	private MovieCatalog actionCatalog; 
	
	private MovieCatalog comedyCatalog; 
	
	@Autowired 
	public void setComedyCatalog(@Genre("Comedy") MovieCatalog comedyCatalog) {
		this.comedyCatalog = comedyCatalog; 
	} 
	
	// ... 
}
```

%%Kotlin%%
```
class MovieRecommender { 
	@Autowired 
	@Genre("Action") 
	private lateinit var actionCatalog: MovieCatalog 
	
	private lateinit var comedyCatalog: MovieCatalog 
	
	@Autowired 
	fun setComedyCatalog(@Genre("Comedy") comedyCatalog: MovieCatalog) { 
		this.comedyCatalog = comedyCatalog 
	} 
	
	// ... 
}
```

%%XML%%
```
<?xml version="1.0" encoding="UTF-8"?> 
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context 
		https://www.springframework.org/schema/context/spring-context.xsd"> 
		
	<context:annotation-config/> 
	
	<bean class="example.SimpleMovieCatalog"> 
		<qualifier type="Genre" value="Action"/> 
		<!-- inject any dependencies required by this bean --> 
	</bean> 
	
	<bean class="example.SimpleMovieCatalog"> 
		<qualifier type="example.Genre" value="Comedy"/> 
		<!-- inject any dependencies required by this bean --> 
	</bean> <bean id="movieRecommender" class="example.MovieRecommender"/> 
</beans>
```

#### Определение квалификатора для офлайн использования

Создание:

%%Java%%
```
@Target({ElementType.FIELD, ElementType.PARAMETER}) 
@Retention(RetentionPolicy.RUNTIME) 
@Qualifier 
public @interface Offline { }
```

%%Kotlin%%
```
@Target(AnnotationTarget.FIELD, AnnotationTarget.VALUE_PARAMETER) 
@Retention(AnnotationRetention.RUNTIME) 
@Qualifier 
annotation class Offline
```

Применение:

%%Java%%
```
public class MovieRecommender { 
	@Autowired 
	@Offline 
	private MovieCatalog offlineCatalog; 
	
	// ... 
}
```

%%Kotlin%%
```
class MovieRecommender { 
	@Autowired 
	@Offline 
	private lateinit var offlineCatalog: MovieCatalog 
	
	// ... 
}
```

%%XML%%
```
<bean class="example.SimpleMovieCatalog"> 
	<qualifier type="Offline"/> 
	<!-- inject any dependencies required by this bean --> 
</bean>
```

#### Определение квалификатора с несколькими атрибутами

%%Java%%
```
@Target({ElementType.FIELD, ElementType.PARAMETER}) 
@Retention(RetentionPolicy.RUNTIME) 
@Qualifier 
public @interface MovieQualifier { 
	String genre(); 
	Format format(); 
}
```

%%Kotlin%%
```
@Target(AnnotationTarget.FIELD, AnnotationTarget.VALUE_PARAMETER) 
@Retention(AnnotationRetention.RUNTIME) 
@Qualifier 
annotation class MovieQualifier(val genre: String, val format: Format)
```

Где Format это:

%%Java%%
```
public enum Format { VHS, DVD, BLURAY }
```

%%Kotlin%%
```
enum class Format { VHS, DVD, BLURAY }
```

Применение:

%%Java%%
```
public class MovieRecommender { 
	@Autowired 
	@MovieQualifier(format=Format.VHS, genre="Action") 
	private MovieCatalog actionVhsCatalog; 
	
	@Autowired 
	@MovieQualifier(format=Format.VHS, genre="Comedy") 
	private MovieCatalog comedyVhsCatalog; 
	
	@Autowired 
	@MovieQualifier(format=Format.DVD, genre="Action") 
	private MovieCatalog actionDvdCatalog; 
	
	@Autowired 
	@MovieQualifier(format=Format.BLURAY, genre="Comedy") 
	private MovieCatalog comedyBluRayCatalog; 
	
	// ... 
}
```

%%Kotlin%%
```
class MovieRecommender { 
	@Autowired 
	@MovieQualifier(format = Format.VHS, genre = "Action") 
	private lateinit var actionVhsCatalog: MovieCatalog 
	
	@Autowired 
	@MovieQualifier(format = Format.VHS, genre = "Comedy") 
	private lateinit var comedyVhsCatalog: MovieCatalog 
	
	@Autowired 
	@MovieQualifier(format = Format.DVD, genre = "Action") 
	private lateinit var actionDvdCatalog: MovieCatalog 
	
	@Autowired 
	@MovieQualifier(format = Format.BLURAY, genre = "Comedy") 
	private lateinit 
	var comedyBluRayCatalog: MovieCatalog 
	
	// ... 
}
```

%%XML%%
```
<?xml version="1.0" encoding="UTF-8"?> 
<beans xmlns="http://www.springframework.org/schema/beans" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context" 
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
		https://www.springframework.org/schema/beans/spring-beans.xsd 
		http://www.springframework.org/schema/context 
		https://www.springframework.org/schema/context/spring-context.xsd"> 
		
	<context:annotation-config/> 
	
	<bean class="example.SimpleMovieCatalog"> 
		<qualifier type="MovieQualifier"> 
			<attribute key="format" value="VHS"/> 
			<attribute key="genre" value="Action"/> 
		</qualifier> 
		<!-- inject any dependencies required by this bean --> 
	</bean> 
	
	<bean class="example.SimpleMovieCatalog"> 
		<qualifier type="MovieQualifier"> 
			<attribute key="format" value="VHS"/> 
			<attribute key="genre" value="Comedy"/> 
		</qualifier> 
		<!-- inject any dependencies required by this bean --> 
	</bean> 
	
	<bean class="example.SimpleMovieCatalog"> 
		<meta key="format" value="DVD"/> 
		<meta key="genre" value="Action"/> 
		<!-- inject any dependencies required by this bean --> 
	</bean> 
	
	<bean class="example.SimpleMovieCatalog"> 
		<meta key="format" value="BLURAY"/> 
		<meta key="genre" value="Comedy"/> 
		<!-- inject any dependencies required by this bean --> 
	</bean> 
</beans>
```