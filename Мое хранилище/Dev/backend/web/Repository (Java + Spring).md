
- `JpaRepository<User, Long>` - предпочтительней, уже содержит в себе CRUD, pagination, flushing.

- `@Repository` - помечает класс как репозиторий.

- `Optional` - всегда возвращать, если результат один элемент.

- `Page`/`Slice` - использовать для групп элементов, не нужно использовать списки.

- `Specification` - использовать для динамических запросов.

- `@SQLRestriction("deleted = false")` в сущности и `@Modifying` в репозитории - используется для мягкого удаления.

Пример тестирования:

```
@DataJpaTest
@Import(UserRepository.class)
class UserRepositoryTest {

	@Autowired 
	private UserRepository userRepository;
	
	@Autowired 
	private TestEntityManager entityManager;
	
	@Test
	void findByEmailShouldReturnUser() {
	
		User user = new User("ivan", "ivan@mail.com", true, new Role("USER"));
		
		entityManager.persist(user);
		entityManager.flush();
		
		Optional<User> found = userRepository.findByEmail("ivan@mail.com");
		
		assertThat(found).isPresent();
		assertThat(found.get().getEmail()).isEqualTo("ivan@mail.com");
	}
	
	@Test
	void pageShouldReturnCorrectSize() {
	
		Page<User> page = userRepository.findAll(PageRequest.of(0, 10));
		
		assertThat(page.getTotalElements()).isEqualTo(15);
		assertThat(page.getContent()).hasSize(10);
	}
}
```