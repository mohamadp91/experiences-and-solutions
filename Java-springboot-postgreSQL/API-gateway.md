## micro service API gateway
There are different ways to send a request like a what we did in integration-testing.
Another way is a little different.
___
First inject an instance of **_RestTemplate_** and **_HttpHeaders_** that explained in integration-testing section. 
```
@Autowired
private RestTemplate restTemplate;

HTTPHeaders headers = new HttpHeaders();
```

Now, to send a request in the json form you need below code:

```
headers.setAccept(Collections.singletonList(MediaType.APPLICATION_JSON));
```

Then create an instance of HTTPEntity that explained in integration-testing section like this:

```
HttpEntity<ModelClass> entity = new HttpEntity<ModelClass>(user,headers);
```

**_ModelClass_** is your data that you want to send that in form a class(Data in here).

To send a sample request look below code:
```
 return restTemplate.exchange(
                url + "/users",
                HttpMethod.POST,
                entity,
                ModelClass.class);
```

Your method must have an annotation named **_@CrossOrigin_** and return the **_ResponseEntity<DataClass>**_ object.
Furthermore, to receive a request in json body, you can use the **_@RequestBody_**.
 
### WebClient instead of RestTemplate
First add the dependency :
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
    <scope>test</scope>
</dependency>
```
then you can add webClient to component with :

```
public WebClient getWebClient(){
    return WebClient.create();
```
```
    public Mono<UserModel> addUser(@RequestBody UserModel user) {

        return webClient
                .post()
                .uri(url + "/users")
                .header(HttpHeaders.CONTENT_TYPE,MediaType.APPLICATION_JSON_VALUE)
                .body(Mono.just(user),UserModel.class)
                .retrieve()
                .bodyToMono(UserModel.class);
    }

```

* Call the retrieve() or exchange() method. The retrieve() method directly performs the HTTP request and retrieve 
  the response body. The exchange() method returns ClientResponse having the response status and headers. We can get 
  the response body from ClientResponse instance.
  

* Mono when you want an object
* Flux When you want a list of object

Another implementation:

```
@Bean
public WebClient.Builder getWebBuilder(){
    return WebClient.builder()
}
```

in controller:
```
 @Qualifier("getWebClient")
    @Autowired
    WebClient.Builder webClient;
```
and api:
```
@CrossOrigin
public YourModelClass add(@RequestBody YourModelClass user){

  return webClient
               .build()
               .post()
               .uri(url)
               .header(HttpHeaders.CONTENT_TYPE,MediaType.APPLICATION_JSON_VALUE)
               .body(Mono.just(user),UserModel.class)
               .retrieve()
               .bodyToMono(UserModel.class)
               .block();
}
```