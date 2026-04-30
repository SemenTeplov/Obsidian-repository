
Требуется библиотека `spring-boot-starter-validation`.

- Предпочтительней использовать `record` или `final`.

- Один DTO на сценарий.

- Валидация только на входящих DTO, на исходящих не нужна.

- Можно использовать JSON форматы: `@JsonInclude`, `@JsonProperty,` `@JsonFormat.`

Пример тестирования:

```
@Validated
@SpringBootTest
class CreateUserRequestTest {

	@Autowired 
	private Validator validator;
	
	@Test
	void invalidEmailShouldFail() {
		var request = new CreateUserRequest("user", "bad-email", LocalDate.now(), true);
		var violations = validator.validate(request);
		
		assertThat(violations).isNotEmpty();
	}
	
	@Test
	void validRequestShouldPass() {
		var request = new CreateUserRequest("validUser", "user@mail.com", LocalDate.now(), true);
		var violations = validator.validate(request);
		
		assertThat(violations).isEmpty();
	}
}
```
