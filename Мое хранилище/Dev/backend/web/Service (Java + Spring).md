
- `@Service` - отмечает сервис.

- `@RequiredArgsConstructor` - позволяет автоматически инициализировать поля с модификаторами `final` и `private`.

- `@Slf4j` - позволяет писать логи.

- Избегание изменяемых полей.

- `@Transactional` - используется на методах, позволяет определять границы транзакции.

- Необходимо проводить бизнес валидацию.

- `@Cacheable("users")`, `@CacheEvict`, `@CachePut` - использование для кэширования.

- `@Async` + `CompletableFuture` (требует `@EnableAsync`) - для асинхронного взаимодействия.

- `@SQLRestriction("deleted = false")` + `@Modifying @Query` - мягкое удаление.

- `@Version private Long version;` в Entity - оптимистическая блокировка.

Пример модульного тестирования:

```
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
	
	@Mock 
	private UserRepository userRepository;
	
	@Mock 
	private UserMapper userMapper;
	
	@Mock 
	private EmailNotificationService emailService;
	
	@InjectMocks 
	private UserService userService;
	
	@Test
	void createShouldThrowWhenEmailExists() {
	
		when(userRepository.existsByEmail("test@mail.com")).thenReturn(true);
		
		var request = new CreateUserRequest("user", "test@mail.com", LocalDate.now(), true);
		
		assertThatThrownBy(() -> userService.create(request)).isInstanceOf(EmailAlreadyExistsException.class);
		
		verify(userRepository, never()).save(any());
	}
	
	@Test
	void createShouldSaveAndReturnResponse() {
	
		when(userRepository.existsByEmail(any())).thenReturn(false);
		when(userMapper.toEntity(any())).thenReturn(new User());
		when(userRepository.save(any())).thenReturn(new User(1L, "test", "test@mail.com", true, null));
		when(userMapper.toResponse(any())).thenReturn(new UserResponse(1L, "test", "test@mail.com", null, null));
		
		var result = userService.create(new CreateUserRequest("test", "test@mail.com", LocalDate.now(), true));
		
		assertThat(result.id()).isEqualTo(1L);
		
		verify(emailService).sendWelcomeEmail("test@mail.com", "test");
	}
}
```

Пример интеграционного теста

```
@DataJpaTest
@Import(UserService.class)
class UserServiceIntegrationTest {

	@Autowired 
	private UserService userService;
	
	@Autowired 
	private UserRepository userRepository;
	
	// тесты с @Transactional + TestEntityManager
}
```
