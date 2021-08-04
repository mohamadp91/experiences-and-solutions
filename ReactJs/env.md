## env
To externalize url , etc in js libraries and springboot project you can use environments.
___
First create .env file in root directory of your project.
Now create a constant in that. Your constant must begin with **_REACT_APP_** .
```
REACT_APP_API_HOST=http://localhost:8080
```
Now import env in your file like this:
```
import env from "@beam-australia/react-env"
```

```
`${env("API_HOST")}`
```

___

To externalize url in springboot project, put the url in application.properties like this:
```
[project-name].url = http//localhost:8080
```
To access the url in your project:
```
@Value("${name in application.properties}")
private String url;
```