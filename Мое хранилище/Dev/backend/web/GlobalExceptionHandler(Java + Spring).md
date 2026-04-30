
- `@RestControllerAdvice` - помечает класс как глобальный обработчик ошибок

- `@Slf4j` - позволяет писать логи

- `ProblemDetail` - рекомендуется для возвращения

- `type` и `timestamp` - рекомендуется добавлять для удобства

- `MethodArgumentNotValidException` - обрабатывает исключения от валидации

- `@ExceptionHandler(MethodArgumentNotValidException.class)` - описание класса ошибки

- `@ResponseStatus(HttpStatus.NOT_FOUND)` - ставится на кастомнное исключение

Пример тестирования:

```
@WebMvcTest(controllers = UserController.class)
class GlobalExceptionHandlerTest {
	
	@Autowired 
	private MockMvc mockMvc;
	
	@MockBean 
	private UserService userService;
	
	@Test
	void validationErrorShouldReturn400WithProblemDetail() throws Exception {
	
		String badJson = """
			{ "username": "ab", "email": "invalid" }
		""";
		
		mockMvc.perform(post("/api/v1/users")
			.contentType(MediaType.APPLICATION_JSON)
			.content(badJson))
			.andExpect(status().isBadRequest())
			.andExpect(content().contentType("application/problem+json"))
			.andExpect(jsonPath("$.status").value(400))
			.andExpect(jsonPath("$.title").value("Validation failed"))
			.andExpect(jsonPath("$.errors").isArray())
			.andExpect(jsonPath("$.errors[0].field").value("username"));
	}
	  
	@Test
	void unhandledExceptionShouldReturn500() throws Exception {
		when(userService.getById(any())).thenThrow(new RuntimeException("DB crash"));
		  
		mockMvc.perform(get("/api/v1/users/999"))
			.andExpect(status().isInternalServerError())
			.andExpect(jsonPath("$.title").value("Internal server error"))
			.andExpect(jsonPath("$.detail").doesNotExist());
	}
}
```