
- Производит маппинг сущностей  в DTO

- Один адаптер, одна технология

- Технологии находятся только здесь

- Adapter взаимодействует только с UseCase


Пример тестирования входящего adapter:

```
@WebMvcTest(UserController.class)
class UserControllerTest {

	@Autowired 
	private MockMvc mockMvc;
	
	@MockBean 
	private CreateUserUseCase createUserUseCase;
	
	@Test
	void shouldCallUseCaseAndReturn201() throws Exception {
	
		when(createUserUseCase.execute(any())).thenReturn(new UserCreatedResult(1L, "ivan", "ivan@mail.com"));
		
		mockMvc.perform(post("/api/v1/users")
			.contentType(MediaType.APPLICATION_JSON)
			.content("""{"username":"ivan","email":"ivan@mail.com","role":"USER"}"""))
			.andExpect(status().isCreated())
			.andExpect(jsonPath("$.id").value(1L));
	}
}
```

Пример тестирования исходящего adapter:

```
@DataJpaTest
@Import(JpaUserRepository.class)
class JpaUserRepositoryTest {

	@Autowired 
	private JpaUserRepository repository;
	
	@Autowired 
	private TestEntityManager em;
	
	@Test
	void shouldSaveAndReturnDomainUser() {
	
		User domainUser = User.create("ivan", "ivan@mail.com", "USER");
		User saved = repository.save(domainUser);
		
		assertThat(saved.getId()).isNotNull();
		assertThat(saved.getEmail().value()).isEqualTo("ivan@mail.com");
	}
}
```