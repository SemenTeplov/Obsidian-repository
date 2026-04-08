---
date: 20-03-2026
tags:
  - IT
  - Development
  - JAVA
  - Spring
status: ready
sticker: lucide//file-check
---

| Под темы    | Заметка                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Определение | @Autowired — это аннотация в фреймворке [[Заметки/Spring Framework.md\|Spring Framework]] (начиная с версии 2.5), которая реализует механизм [[Заметки/Dependency Injection.md\|Dependency Injection]] (DI) — внедрение зависимостей.<br>Она позволяет Spring‑контейнеру автоматически:<br>- находить нужные бины (объекты) в контексте приложения;<br>- внедрять их в поля, сеттеры или конструкторы класса.<br>Таким образом устраняется необходимость явного конфигурирования связей между компонентами в XML‑файлах или Java‑конфигурации. |

#### Применение через конструктор

> [!NOTE]
> Не обязательно применять к конструктору, если он используется один.

%% Java %%
```
public class MovieRecommender { 
	private final CustomerPreferenceDao customerPreferenceDao; 

	@Autowired 
	public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) { 
		this.customerPreferenceDao = customerPreferenceDao; 
	} 
	
	// ... 
}
```

%% Kotlin %%
```
class MovieRecommender 
	@Autowired 
	constructor( private val customerPreferenceDao: CustomerPreferenceDao)
```

#### Применение через Setter

%% Java %%
```
public class SimpleMovieLister { 
	private MovieFinder movieFinder; 
	
	@Autowired 
	public void setMovieFinder(MovieFinder movieFinder) { 
		this.movieFinder = movieFinder; 
	} 
	
	// ... 
}
```

%% Kotlin %%
```
class SimpleMovieLister { 
	@set:Autowired 
	lateinit var movieFinder: MovieFinder 
	
	// ... 
}
```

#### Применение через метод

%% Java %%
```
public class MovieRecommender { 
	private MovieCatalog movieCatalog; 
	private CustomerPreferenceDao customerPreferenceDao; 
	
	@Autowired 
	public void prepare(
			MovieCatalog movieCatalog, 
			CustomerPreferenceDao customerPreferenceDao) { 
		this.movieCatalog = movieCatalog; 
		this.customerPreferenceDao = customerPreferenceDao; 
	} 
	
	// ... 
}
```

%% Kotlin %%
```
class MovieRecommender { 
	private lateinit var movieCatalog: MovieCatalog 
	private lateinit var customerPreferenceDao: CustomerPreferenceDao 
	
	@Autowired 
	fun prepare(
			movieCatalog: MovieCatalog, 
			customerPreferenceDao: CustomerPreferenceDao) { 
		this.movieCatalog = movieCatalog 
		this.customerPreferenceDao = customerPreferenceDao 
	} 
	
	// ... 
}
```

#### Совмещение применения через поля и конструктор

%% Java %%
```
public class MovieRecommender { 
	private final CustomerPreferenceDao customerPreferenceDao; 
	
	@Autowired
	private MovieCatalog movieCatalog; 
	
	@Autowired 
	public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) { 
		this.customerPreferenceDao = customerPreferenceDao; 
	} 
	
	// ... 
}
```

%% Kotlin %%
```
class MovieRecommender 
	@Autowired 
	constructor(private val customerPreferenceDao: CustomerPreferenceDao) { 
		
		@Autowired 
		private lateinit var movieCatalog: MovieCatalog 
		
		// ... 
	}
```

#### Применение через массив

> [!NOTE]
> Внедряет все подходящие бины.

%% Java %%
```
public class MovieRecommender { 
	@Autowired 
	private MovieCatalog[] movieCatalogs; 
	
	// ... 
}
```

%% Kotlin %%
```
class MovieRecommender { 
	@Autowired 
	private lateinit var movieCatalogs: Array<MovieCatalog> 
	
	// ... 
}
```

#### Применение через коллекцию

%% Java %%
```
public class MovieRecommender { 
	private Set<MovieCatalog> movieCatalogs; 
	
	@Autowired 
	public void setMovieCatalogs(Set<MovieCatalog> movieCatalogs) { 
		this.movieCatalogs = movieCatalogs; 
	} 
	
	// ... 
}
```

%% Kotlin %%
```
class MovieRecommender { 
	@Autowired 
	lateinit var movieCatalogs: Set<MovieCatalog> 
	
	// ... 
}
```

#### Применение через Map

> [!NOTE]
> Ключом будет название бина, значением сам бин.

%% Java %%
```
public class MovieRecommender { 
	private Map<String, MovieCatalog> movieCatalogs; 
	
	@Autowired 
	public void setMovieCatalogs(Map<String, MovieCatalog> movieCatalogs) { 
		this.movieCatalogs = movieCatalogs; 
	} 
	
	// ... 
}
```

%% Kotlin %%
```
class MovieRecommender { 
	@Autowired 
	lateinit var movieCatalogs: Map<String, MovieCatalog> 
	
	// ... 
}
```

#### Определение не обязательного внедрения

%% Java %%
```
public class SimpleMovieLister { 
	private MovieFinder movieFinder; 
	
	@Autowired(required = false) 
	public void setMovieFinder(MovieFinder movieFinder) { 
		this.movieFinder = movieFinder; 
	} 
	
	// ... 
}
```

%% Kotlin %%
```
class SimpleMovieLister { 
	@Autowired(required = false) 
	var movieFinder: MovieFinder? = null 
	
	// ... 
}
```

#### Применение через Optional

%% Java %%
```
public class SimpleMovieLister { 
	@Autowired 
	public void setMovieFinder(Optional<MovieFinder> movieFinder) { ... } 
}
```

#### Применение с @Nullable

%% Java %%
```
public class SimpleMovieLister { 
	@Autowired 
	public void setMovieFinder(@Nullable MovieFinder movieFinder) { ... } 
}
```


%% Kotlin %%
```
class SimpleMovieLister { 
	@Autowired var movieFinder: MovieFinder? = null 
	
	// ... 
}
```

#### Внедрение контекста

%% Java %%
```
public class MovieRecommender { 
	@Autowired 
	private ApplicationContext context; 
	
	public MovieRecommender() { } 
	
	// ... 
}
```

%% Kotlin %%
```
class MovieRecommender { 
	@Autowired 
	lateinit var context: ApplicationContext 
	
	// ... 
}
```