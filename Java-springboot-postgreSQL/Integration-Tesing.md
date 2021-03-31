## Integration Testing

tests for:
```
 @PostMapping(value = "/users")
    public something addUser(@RequestParam String id, @RequestParam String name, @RequestParam String emailAddress) {
        return something;
    }
```
```
@RunWith(SpringRunner.class)
@SpringBootTest(classes = UserManagementSystemApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class ApplicationIntegrationTests {


    @LocalServerPort
    private int port;

    @Autowired
    private TestRestTemplate restTemplate;

    @Autowired
    private UserRepository userRepository;

    HttpHeaders headers = new HttpHeaders();
    
     HttpEntity<?> entity = new HttpEntity<>(headers);

        UserModel user = new UserModel("1", "ali", new Date().toString(), "hello@gamil.com");

        UriComponentsBuilder builder = UriComponentsBuilder.fromHttpUrl("http://localhost:" + port + "/users")
                .queryParam("id", user.getId())
                .queryParam("name",user.getName())
                .queryParam("emailAddress",user.getEmailAddress());


        HttpEntity<String> response = restTemplate.exchange(
                builder.build().encode().toUri(),
                HttpMethod.POST,
                entity,
                String.class);

        assertTrue(userRepository.existsById(user.getId()));
```

### @SpringBootTest(classes = MainClass, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
The @SpringBootTest annotation tells Spring Boot to look for a main configuration class (one with @SpringBootApplication,
for instance) and use that to start a Spring application context. 

### @LocalServerPort
Annotation at the field or method/constructor parameter level that injects the HTTP port that got allocated at runtime. Provides a convenient alternative for @Value("${local.server.port}").

### TestRestTemplate
Convenient alternative of RestTemplate that is suitable for integration tests. They are fault tolerant, and optionally 
can carry Basic authentication headers. If Apache Http Client 4.3.2 or better is available (recommended) it will be used as the client,
and by default configured to ignore cookies and redirects.

### HTTPHeaders
A data structure representing HTTP request or response headers, mapping String header names to a list of String values,
also offering accessors for common application-level data types. 

### HTTPEntity 
Represents an HTTP request or response entity, consisting of headers and body. 

### UriComponentsBuilders
The builder works in conjunction with the UriComponents class – an immutable container for URI components.
A new UriComponentsBuilder class helps to create UriComponents instances by providing fine-grained control over all 
aspects of preparing a URI including construction, expansion from template variables, and encoding.