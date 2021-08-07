# Spring-Boot-Maven

#### Dependency

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-oauth2-client</artifactId>
    </dependency>
</dependencies>
```
#### Create Controller & Security Config

`OAuth2Controller`
```java
@RestController
public class OAuth2Controller {

    @GetMapping("/")
    public String helloWorld() {
        return "Hello World!";
    }

    @GetMapping("/restricted")
    public String restricted() {
        return "You have logged in!";
    }
}
```

`SecurityConfig`
```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception{
        http
                .antMatcher("/**").authorizeRequests()
                .antMatchers("/").permitAll()
                .anyRequest().authenticated()
                .and()
                .oauth2Login(); // redirect the user to Google in this case
    }
}
```

`.antMatchers("/").permitAll()`: we can access `localhost:8080` without login.

On the other hand, for path `localhost:8080/restricted`, we need to login to access that page

So, after the user logged in their Google account, they can thus see the return value of `localhost:8080/restricted`

But, before that, we need to create a client for login.

#### Create Client for Login

1. Go to [Google Console](https://console.cloud.google.com/)
2. Choose `APIs & Service`
3. Choose `Credentials`
4. Choose `Create Credentials`
5. Choose `OAuth client ID`
6. Choose `Web Application`
7. Put `http://localhost:8080/login/oauth2/code/google` inside the block of `Authorised redirect URIs`
8. Record the **client ID** and **client Secret** and put them in `application.properties`
    ```properties
    spring.security.oauth2.client.registration.google.client-id={Your Client ID}
    spring.security.oauth2.client.registration.google.client-secret={Your Client Secret}
    ```

Then, we can run the application and try it on web browser.

#### Run

1. Input `http://localhost:8080/`, we can access the return value

2. Input `http://localhost:8080/restricted`, we need to login in to our Google account first to see the return value




---