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

| Под темы    | Заметка                                                                |
| ----------- | ---------------------------------------------------------------------- |
| Определение | @Primary - аннотация, определяющая приоритетный бин при авто инъекции. |

%%Java%%
```
@Configuration 
public class MovieConfiguration { 
	@Bean 
	@Primary 
	public MovieCatalog firstMovieCatalog() { ... } 
	
	@Bean 
	public MovieCatalog secondMovieCatalog() { ... } 
	
	// ... 
}
```

%%Kotlin%%
```
@Configuration 
class MovieConfiguration { 
	@Bean 
	@Primary 
	fun firstMovieCatalog(): MovieCatalog { ... } 
	
	@Bean 
	fun secondMovieCatalog(): MovieCatalog { ... } 
	
	// ... 
}
```

Альтернатива:

%%Java%%
```
@Configuration 
public class MovieConfiguration { 
	@Bean 
	public MovieCatalog firstMovieCatalog() { ... } 
	
	@Bean 
	@Fallback 
	public MovieCatalog secondMovieCatalog() { ... } 
	
	// ... 
}
```

%%Kotlin%%
```
@Configuration 
class MovieConfiguration { 
	@Bean 
	fun firstMovieCatalog(): MovieCatalog { ... } 
	
	@Bean 
	@Fallback 
	fun secondMovieCatalog(): MovieCatalog { ... } 
	
	// ... 
}
```

Применение:

%%Java%%
```
public class MovieRecommender { 
	@Autowired 
	private MovieCatalog movieCatalog; 
	// ... 
}
```

%%Kotlin%%
```
class MovieRecommender { 
	@Autowired 
	private lateinit var movieCatalog: MovieCatalog 
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
	
	<bean class="example.SimpleMovieCatalog" primary="true"> 
		<!-- inject any dependencies required by this bean --> 
	</bean> 
	
	<bean class="example.SimpleMovieCatalog"> 
		<!-- inject any dependencies required by this bean --> 
	</bean> 
	
	<bean id="movieRecommender" class="example.MovieRecommender"/> 
</beans>
```