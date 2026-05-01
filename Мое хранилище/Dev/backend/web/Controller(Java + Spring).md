
- `@RestController` - помечает класс как контроллер

- `@RequestMapping("/api/v1/users")` - общий шаблон запроса

- `@RequiredArgsConstructor` - позволяет инициализировать поля через конструктор с модификаторами `private` и `final`

- `@Validated` - позволяет проводить валидацию запросов

- `@PostMapping`, `@GetMapping`, `@PatchMapping`, `@DeleteMapping` - определяет ожидаемый http метод

- `@Valid` - позволяет валидировать тело запроса

- `@RequestBody` - принимает тело запроса, предпочтительно использовать DTO

- `@ResponseStatus`, `ResponseEntity` - определяет статус ответа.

- `@PageableDefault(size = 20, sort = "id,asc")` - предпочтительно принимать страницы, а не списки

Пример тестирования:

```
@WebMvcTest(UserController.class)
class UserControllerTest {
  
	@Autowired 
	private MockMvc mockMvc;
	
	@MockBean 
	private UserService userService;
	
	@Test
	void createShouldReturn201AndResponse() throws Exception {
	
		String requestJson = """
			{ "username": "ivan", "email": "ivan@mail.com" }
		""";
		
		when(userService.create(any())).thenReturn(new UserResponse(1L, "ivan", "ivan@mail.com", null, "USER"));
		
		mockMvc.perform(post("/api/v1/users")
			.contentType(MediaType.APPLICATION_JSON)
			.content(requestJson))
			.andExpect(status().isCreated())
			.andExpect(jsonPath("$.id").value(1L))
			.andExpect(jsonPath("$.email").value("ivan@mail.com"));
	}
	
	@Test
	void invalidEmailShouldReturn400() throws Exception {
	
		String badJson = """
			{ "username": "ivan", "email": "not-an-email" }
		""";
		
		mockMvc.perform(post("/api/v1/users")
			.contentType(MediaType.APPLICATION_JSON)
			.content(badJson))
			.andExpect(status().isBadRequest())
			.andExpect(jsonPath("$.detail").value("Validation failed"));
	}
}
```