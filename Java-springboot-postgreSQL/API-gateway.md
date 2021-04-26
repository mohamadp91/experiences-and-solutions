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

Then create a instance of HTTPEntity that explained in integration-testing section like this:

```
HttpEntity<DataClass> entity = new HttpEntity<DataClass>(user,headers);
```

**_DataClass_** is your data that you want to send that in form a class(Data in here).

To send a sample request look below code:
```
 return restTemplate.exchange(
                url + "/users",
                HttpMethod.POST,
                entity,
                DataClass.class);
```

Your method must have an annotation named **_@CrossOrigin_** and return the **_ResponseEntity<DataClass>**_ object.
Furthermore, to receive a request in json body, you can use the **_@RequestBody_**.
 
