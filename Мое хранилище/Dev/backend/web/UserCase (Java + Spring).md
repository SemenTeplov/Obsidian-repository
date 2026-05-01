
- Исполняет только одну операцию

- Должен присутствовать интерфейс для четкого ограничения

- `@RequiredArgsConstructor` - используется для инициализации полей с модификаторами `final` и `private`

- `@Transactional` - ограничивает транзакцию

- `DTO` или `Commands` - предназначены для принятия и отправки

Пример тестирования:

```
@ExtendWith(MockitoExtension.class)
class CreateUserUseCaseImplTest {

	@Mock 
	private UserRepository userRepository;
	
	@Mock 
	private EmailNotificationPort emailPort;
	
	@Mock 
	private UserDomainMapper mapper;
	
	@InjectMocks 
	private CreateUserUseCaseImpl useCase;
	
	@Test
	void shouldCreateUserAndSendEmail() {
	
		var cmd = new CreateUserCommand("ivan", "ivan@mail.com", "USER");
		var domainUser = User.create("ivan", "ivan@mail.com", "USER");
		var savedUser = domainUser.assignId(1L);
		
		when(userRepository.existsByEmail(cmd.email())).thenReturn(false);
		when(userRepository.save(any())).thenReturn(savedUser);
		doNothing().when(emailPort).sendWelcomeEmail(any(), any());
		
		var result = useCase.execute(cmd);
		
		assertThat(result.userId()).isEqualTo(1L);
		
		verify(userRepository).save(any());
		verify(emailPort).sendWelcomeEmail("ivan@mail.com", "ivan");
	}
	
	@Test
	void shouldThrowWhenEmailExists() {
	
		var cmd = new CreateUserCommand("ivan", "ivan@mail.com", "USER");
		
		when(userRepository.existsByEmail(cmd.email())).thenReturn(true);
		
		assertThatThrownBy(() -> useCase.execute(cmd))
			.isInstanceOf(EmailAlreadyExistsException.class);
		
		verify(userRepository, never()).save(any());
	}
}
```