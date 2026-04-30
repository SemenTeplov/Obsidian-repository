
Используются библиотеки: `mapstruct`, `lombok`.

- `injectionStrategy = CONSTRUCTOR` - указывается в Mapper, безопасно для `final` полей.

- `unmappedTargetPolicy = ERROR` - указывается в Mapper, ловит забытые поля, выбрасывает исключение.

- `componentModel = "spring"` - указывается в Mapper, необходимо для работы со Spring.

- `@BeanMapping(nullValuePropertyMappingStrategy = IGNORE)` - игнорирование Null при обновлении.

- `@MappingTarget` - обновляет значение, не создает новое.

- `Mappers.getMapper()` - не допустим, рекомендуется пользоваться `@Autowired`.

- `@Mapper(uses = {RoleConverter.class})` - позволяет интегрировать сложную логику.

- `@Mapping(target = "password", ignore = true)` - необходимо избегать передачи чувствительных данных.

- `default` методы или `@Named("custom")` - для маппинга сложных типов.

- `@Mapping(target = "x", source = "y")` - явное связывание полей.

- `@Mapping(target = "x", ignore = true)` - исключение поля из маппинга.



Пример модульного тестирования:

```
@ExtendWith(MockitoExtension.class)
class UserMapperTest {

	private final UserMapper mapper = Mappers.getMapper(UserMapper.class);
	
	@Test
	void shouldMapEntityToResponse() {
	
		User entity = new User(1L, "ivan", "ivan@mail.com", true, new Role("ADMIN"));
		UserResponse response = mapper.toResponse(entity);
		
		assertThat(response.id()).isEqualTo(1L);
		assertThat(response.username()).isEqualTo("ivan");
		assertThat(response.role()).isEqualTo("ADMIN");
	}
}
```

Пример интеграционного теста:

```
@SpringBootTest
class UserMapperIntegrationTest {
	  
	@Autowired 
	private UserMapper mapper;
	
	@Test
	void springInjectionWorks() {
		assertThat(mapper).isNotNull();
	}
}
```
