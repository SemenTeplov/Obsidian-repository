---
date: 12-02-2026
tags:
  - IT
  - JAVA
  - Spring
status: editing
sticker: lucide//file
---
### Создание объекта (конструктор)
---
#### XML-конфигурация

```
<bean id="myBean" class="com.example.MyBean"> 
	<constructor-arg value="value1"/> 
	<constructor-arg ref="anotherBean"/> 
</bean>
```

- Spring находит класс.
- Ищет конструктор, подходящий под типы аргументов.
- Создает экземпляр, передавая значение и bean в конструктор.

#### Java-конфигурация

```
@Configuration 
public class AppConfig {
	@Bean 
	public MyBean myBean(AnotherBean anotherBean) { 
		return new MyNewton("value1", anotherBean); 
	} 
}
```

- Spring вызывает метод, подставляя зависимости из контекста.
- Метод возвращает новый экземпляр, созданный через конструктор.

#### Конфигурация на основе аннотаций

```
@Component 
public class MyBean { 
	private final AnotherBean anotherBean; 
	
	@Autowired 
	public MyBean(AnotherBean anotherBean) { 
		this.anotherBean = anotherBean; 
	} 
}
```

- Spring сканирует bean'ы (`<context:component-scan>` или `@ComponentScan`).
- Находит помеченный класс с конструктором, требующий другой bean.
- Ищет контекст необходимого bean.
- Создает bean, передавая найденный бин в конструктор.

### Внедрение свойств и зависимостей (autowiring)
---

#### Через конструктор

Зависимости передаются в параметрах конструктора.

```
@Component 
public class OrderService { 
	private final PaymentService paymentService; 
	
	@Autowired 
	public OrderService(PaymentService paymentService) { 
		this.paymentService = paymentService; 
	} 
}
```

#### Через сеттер

Зависимости внедряются через методы-сеттеры.

```
@Component 
public class OrderService { 
	private PaymentService paymentService; 
	
	@Autowired 
	public void setPaymentService(PaymentService paymentService) { 
		this.paymentService = paymentService; 
	} 
}
```

#### Через поле (не рекомендуется)

Зависимости внедряются прямо в поле класса.

```
@Component 
public class OrderService { 
	@Autowired 
	private PaymentService paymentService; 
}
```

### Вызов Aware интерфейсов
---

**Aware‑интерфейсы в Spring** — это набор специальных интерфейсов, позволяющих bean получить доступ к инфраструктурным объектам контейнера (контекст, имя bean, фабрика и др.). Они работают по принципу callback‑механизма: Spring автоматически вызывает соответствующий метод интерфейса при инициализации bean.

- BeanNameAware - позволяет узнать название bean в контейнере.
- BeanFactoryAware - предоставляет доступ в BeanFactory (получение других bean'ов).
- ApplicationContextAware - предоставляет доступ к ApplicationContext.
- EnvironmentAware - дает доступ к Enviropment.
- ResourceLoaderAware - позволяет загружать ресурсы.
- MessageSourceAware - предоставляет доступ к механизму интернационализации.
- ServeletContextAware - предоставляет доступ к ServeletContex.

### Пре-инициализация
---

- Валидация bean - проверка требований к объектам до инициализации
- Динамическая настройка свойств - изменение поля bean в зависимости от профиля и окружения.
- Логирование - записывает информацию о bean перед инициализацией.
- Проксирование - создание прокси объектов ([[Заметки/@Transactional.md|@Transactional]], [[Заметки/@Cacheable.md|@Cacheable]])
- Проверка ограничений - проверка требований к bean.

### Инициализация
---

- `@PostConstruct` - вызывается один раз после создания bean и внедрения всех зависимостей.
- `InitializingBean` - метод afterPropertiesSet() вызывается после установки всех свойств bean.
- `initMethod` - указывает на название метода инициализации.

### Пост-инициализация
---

`BeanPostProcessor` - метод postProcessAfterInitialization вызывается после всех методов инициализации (логирование, проксирование, дополнительная валидация, модификация bean, интеграция с внешними системами).

### Завершение работы
---

- `@PreDestroy` - вызывается перед уничтожением bean.
- `DisposableBean` - метод destroy() вызывается перед удаление bean.
- `destroyMethod` - вызывается перед уничтожением bean

### Вывод
---
