
Для настройки требуются следующие библиотеки:
`spring-boot-starter-data-jpa`, `spring-boot-starter-validation`, `postgresql`, `lombok`

- Сущность должна быть помечена аннотацией: `@Entity`.

- Нужно указать таблицу, к которой привязана сущность: `@Table(name = "users", indexes = { @Index(name = "idx_user_email", columnList = "email", unique = true) })` - индекс ускоряет поиск.

- Для аудита используется `@EntityListeners(AuditingEntityListener.class)` + отдельный конфиг файл 

```
@Configuration 
@EnableJpaAuditing 
public class JpaConfig {} 
```

и над полем `createAt` повесить `@CreatedDate`.

- Для создания геттеров и сеттеров нужно использовать `@Getter`, `@Setter`, а не `@Data`.

- Конструкторы нужно определять с помощью аннотация `@NoArgsConstructor`, `@AllArgsConstructor`.

- Над идентификатором нужно повесить `@Id`, `@GeneratedValue(strategy = GenerationType.IDENTITY)`.

- Ограничения на поля нужно вешать с помощью аннотации `@Column(nullable = false, length = 50)`.

- Связи между таблицами определяются с помощью `@OneToOne`, `@OneToMany`, `@ManyToOne` и `@ManyToMany`.

- Переопределение `hashCode` и `equals` должны ссылаться на неизменяемые и не ленивые поля, либо использовать аннотацию `@EqualsAndHashCode(onlyExplicitlyIncluded = true)`.

- Переопределение `toString`, должно ссылаться на неизменяемые и не ленивые поля.

- Даты должны быть либо `LocalDateTime`, либо `Instant`.