## Unit Testing

tests for : 
```
 @PostMapping(value = "/users")
    public something addUser(@RequestParam String id, @RequestParam String name, @RequestParam String emailAddress) {
        return something;
    }
```
``` 
 @RunWith(SpringRunner.class)
 @WebMvcTest(value = UserController.class)
 class UnitApplicationTest {
    
    @Autowired
    private MockMvc mockMvc;


    @MockBean
    private ControllerClassName controller;

    @Test
    void shouldAddUser() throws Exception {

        Date date = new Date();
        UserModel user = new UserModel("1", "ali", date.toString(), "gmail@gmail.com");


        when(controller.addUser(user.getId(), user.getName(), user.getEmailAddress())).thenReturn(user);

        RequestBuilder request = MockMvcRequestBuilders
                .post("/users")
                .param("id", user.getId())
                .param("name", user.getName())
                .param("emailAddress", user.getEmailAddress())
                .accept(MediaType.APPLICATION_JSON);

        String jsonText = JSONValue.toJSONString(user);

        MvcResult result = this.mockMvc.perform(request)
                .andDo(print())
                .andExpect(status().isOk())
                .andExpect(content().json(jsonText))
                .andReturn();
    }
  }
 ```

### **_@RunWith(SpringRunner.class)_**
The SpringRunner provides support for loading a Spring ApplicationContext 
and having beans @Autowired into your test instance.
It actually does a whole lot more than that (covered in the Spring Reference Manual),
but that's the basic idea.


### **_@WebMvcTest(value = ExampleController.class)_**
@WebMvcTest annotation will load only the controller layer of the application.
This will scan only the @Controller/ @RestController annotation and will not load the fully ApplicationContext.
If there is any dependency in the controller layer (has some dependency to other beans from your service layer),
you need to provide them manually by mocking those objects.

### **_MockMvc_**
@WebMvcTest also auto-configures MockMvc.
MockMVC offers a powerful way to quickly test MVC controllers without needing to start a full HTTP server.

### JSONValue

JsonValue represents an immutable JSON value. A JSON value is one of the following: an object ( JsonObject ), an array ( JsonArray ), a number ( JsonNumber ), a string ( JsonString ), true ( JsonValue. TRUE ), false ( JsonValue. FALSE ), or null ( JsonValue ).
