## Curl Endpoints for SpringBoot Project with PostgreSql as database

to get all users from a method with _**@GetMapping("/users")**_ and returns a json.

`curl -X GET http://localhost:8081/users`

to add a user to a method with _**@PostMapping("/users")**_ and receives prams with _**@RequestParam**_ like below and returns a json. 

`curl -X POST -d 'id'='1' -d 'name'='ali' -d 'emailAddress'='hello@gmail.com' http://localhost:8081/users`

To get a user by id from a method with _**@GetMapping("/user")**_ and receives a param with _**@RequestParam**_ like below and returns a json.

`curl -X GET -G http://localhost:8081/user -d 'id'='1'`

To delete a user by id from a method with _**@DeleteMapping("/users")**_ and receives a param with _**@RequestParam**_ like below and returns a json.:

`curl -X DELETE -G http://localhost:8081/users -d 'id'='1'` 