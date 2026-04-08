---
date: 24-03-2026
tags:
  - IT
  - Development
  - JAVA
  - Spring
status: ready
sticker: lucide//file-check
---

| Под темы                    | Заметка                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Определение                 | Интерфейс org.springframework.context.ApplicationContext представляет собой контейнер IoC Spring и отвечает за создание экземпляров, настройку и сборку бинов. Контейнер получает инструкции по созданию экземпляров, настройке и сборке компонентов, считывая метаданные конфигурации. Метаданные конфигурации могут быть представлены в виде аннотированных классов компонентов, классов конфигурации с фабричными методами, а также в виде внешних XML-файлов или скриптов на языке Groovy. |
| Конфигурационные метаданные | Контейнер Spring IoC использует метаданные конфигурации. Эти метаданные показывают, как разработчик приложения сообщает контейнеру Spring о необходимости создания экземпляров, настройки и сборки компонентов в приложении.                                                                                                                                                                                                                                                                   |
|                             | Сам контейнер Spring IoC полностью независим от формата, в котором фактически записываются метаданные конфигурации. Чаще всего выбирают для своих Spring-приложений конфигурацию на основе Java:                                                                                                                                                                                                                                                                                               |
|                             | Конфигурация на основе аннотаций: определение компонентов с помощью метаданных конфигурации на основе аннотаций в классах компонентов приложения.                                                                                                                                                                                                                                                                                                                                              |
|                             | Конфигурация на основе Java: определяйте компоненты, внешние по отношению к классам приложения, с помощью классов конфигурации на основе Java.                                                                                                                                                                                                                                                                                                                                                 |
| XML и внешние конфигурации  | Метаданные конфигурации на основе XML настраивают эти компоненты как элементы <bean/> внутри элемента <beans/> верхнего уровня.                                                                                                                                                                                                                                                                                                                                                                |
|                             | Для создания экземпляра контейнера необходимо указать путь или пути к файлам ресурсов XML в конструкторе ClassPathXmlApplicationContext, который позволяет контейнеру загружать метаданные конфигурации из различных внешних ресурсов, таких как локальная файловая система, Java CLASSPATH и т. д.                                                                                                                                                                                            |

%%Java%%
```
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

%%Kotlin%%
```
val context = ClassPathXmlApplicationContext("services.xml", "daos.xml")
```

|                                   |                                                                                                                                                                                                                                                                                                                                                              |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Создание метаданные на основе XML | Может быть полезно, если определения компонентов будут распределены по нескольким XML-файлам. Часто каждый отдельный XML-файл конфигурации представляет собой логический уровень или модуль в вашей архитектуре.                                                                                                                                             |
|                                   | Для загрузки определений компонентов из фрагментов XML можно использовать конструктор ClassPathXmlApplicationContext. Этот конструктор принимает несколько расположений ресурсов, как показано в предыдущем разделе. Кроме того, можно использовать один или несколько элементов <import/> для загрузки определений компонентов из другого файла или файлов. |

%%XML%%
```
<beans> 
	<import resource="services.xml"/> 
	<import resource="resources/messageSource.xml"/> 
	
	<bean id="bean1" class="..."/> 
	<bean id="bean2" class="..."/> 
</beans>
```

|                          |                                                                                                                                                                                                                                     |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Использование контейнера | ApplicationContext — это интерфейс для расширенной фабрики, способной вести реестр различных компонентов и их зависимостей. С помощью метода `T getBean(String name, Class<T> requiredType)` можно получить экземпляры компонентов. |
|                          | ApplicationContext позволяет считывать определения компонентов и получать к ним доступ.                                                                                                                                             |

%%Java%%
```
// create and configure beans 
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml"); 

// retrieve configured instance 
PetStoreService service = context.getBean("petStore", PetStoreService.class); 

// use configured instance 
List<String> userList = service.getUsernameList();
```

%%Kotlin%%
```
// create and configure beans 
val context = ClassPathXmlApplicationContext("services.xml", "daos.xml") 

// retrieve configured instance 
val service = context.getBean<PetStoreService>("petStore") 

// use configured instance 
var userList = service.getUsernameList()
```

|     |                                                                                                                                                                                                                                  |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     | При использовании конфигурации Groovy процесс начальной загрузки выглядит почти так же. В нем используется другой класс реализации контекста, поддерживающий Groovy (но также понимающий определения компонентов в формате XML). |

%%Java%%
```
ApplicationContext context = new GenericGroovyApplicationContext("services.groovy", "daos.groovy");
```

%%Kotlin%%
```
val context = GenericGroovyApplicationContext("services.groovy", "daos.groovy")
```

|     |                                                                                                                                          |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------- |
|     | Самый гибкий вариант — это GenericApplicationContext в сочетании с делегатами чтения, например с XmlBeanDefinitionReader для XML-файлов. |

%%Java%%
```
GenericApplicationContext context = new GenericApplicationContext(); 
new XmlBeanDefinitionReader(context).loadBeanDefinitions("services.xml", "daos.xml"); 
context.refresh();
```

%%Kotlin%%
```
val context = GenericApplicationContext() 
XmlBeanDefinitionReader(context).loadBeanDefinitions("services.xml", "daos.xml") 
context.refresh()
```

|     |                                                                       |
| --- | --------------------------------------------------------------------- |
|     | Для файлов Groovy можно также использовать GroovyBeanDefinitionReader |

%%Java%%
```
GenericApplicationContext context = new GenericApplicationContext(); 
new GroovyBeanDefinitionReader(context).loadBeanDefinitions("services.groovy", "daos.groovy"); 
context.refresh();
```

%%Kotlin%%
```
val context = GenericApplicationContext() 
GroovyBeanDefinitionReader(context).loadBeanDefinitions("services.groovy", "daos.groovy") 
context.refresh()
```
