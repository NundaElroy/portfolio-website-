---
title: "Setting up  OAuth2 with Spring Boot"
date: 2025-03-28
description: "Using OAuth2 for authentication in spring-boot"
tags: ["OAuth2", "Spring Boot", "Backend", "Java", "Security"]
---

##  Setting Up OAuth2 with Spring Boot

### 1. Introduction  
  
**OAuth2** is an open [standard](https://auth0.com/intro-to-iam/what-is-oauth-2)
for access delegation  commonly used to grant websites or applications limited access to user information without exposing their passwords. It’s essential for securing APIs, enabling Single Sign-On (SSO), and enhancing user experience by allowing seamless authentication with third-party providers like Google, Facebook, and GitHub.
Spring Boot simplifies OAuth2 implementation by providing built-in support through the Spring Security framework(client server as well as authorisation server). With minimal configuration, developers can quickly integrate OAuth2 authentication, handle user sessions and secure API endpoints.

 

---

### 2. Adding Dependencies  
In your `pom.xml`, add the following dependencies:  

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
---

### 3. Configuring OAuth2 in `application.properties`  
You'll need to register your application with an OAuth2 provider (Google, GitHub, etc.).
so in our case we shall use for Google 
 Here's an example configuration for **Google OAuth**:  


In your .properties files add the following fields
```application.properties
spring.application.name=oauth2demo



# OAuth2.0 Google Credentials
spring.security.oauth2.client.registration.google.client-id=
spring.security.oauth2.client.registration.google.client-secret=
```

 What are client-id and client-secret?
In the context of OAuth2, the client-id and client-secret are credentials used to identify and authenticate your application when it communicates with an OAuth2 provider like Google.

client-id: A public identifier for your application. It's what Google uses to recognize your app during the authentication process.

client-secret: A confidential credential known only to your application and the OAuth2 provider (Google). It acts like a password and must be kept secure. Never expose it in client-side code or public repositories.

When your app attempts to authenticate a user, the client-id and client-secret are sent to the OAuth2 provider. If they match the registered credentials, the provider allows your app to proceed with authentication.

### Where to get these credentials ? 

Obtain OAuth 2.0 client credentials from the Google API Console [guide](https://support.google.com/googleapi/answer/6158849?hl=en).
Obtain you client ID and secret and in place them  as the values for the above respectively 
these are the credentials the Spring Security Oauth2 client uses for authentication with the google servers

---

### 4. Creating the Security Configuration  
Create a `SecurityConfig.java` file to configure OAuth2 authentication:  

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .anyRequest().authenticated()
            )
            .oauth2Login(Customizer.withDefaults())
        return http.build();
    }
}
```
On success you will redirect to the home page of the page you can customize this using the success handler 


### 5. Testing OAuth2 Login  
- Run the Spring Boot app  
- Visit `http://localhost:8080/login`  
- Redirect to Google login page  
- After logging in, Spring Security handles everything  

![login page](/static/images/oauthlogin_firstpost_example.png)

---

### 6. Home Page 


```java HomeController


import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping({"/", ""})
    public String home() {
        return "index.html";
    }
}

```
note place it in the dir below since we arent using any template resolver
```src/main/resources/static/index.htmlindex.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Home</title>
</head>
<body>
<h1>welcome to my app </h1>
<h2> A small price to pay for salvation</h2>
</body>
</html>

```

![home page](/static/images/home_first_post.png)


  

---

Additional Resources
To deepen your understanding of OAuth2 and Spring Security, consider checking out the following resources:

Official Spring Security (Documentation)[https://docs.spring.io/spring-security/reference/servlet/oauth2/index.html#oauth2-client] - Comprehensive guide to using Spring Security for various authentication mechanisms.

Spring Security in Action by Laurentiu Spilca - A highly regarded book that explains Spring Security concepts in detail 

Conclusion
In this article, you've learned how to set up OAuth2 authentication with Spring Boot using Google as the provider. We've covered:

Adding the necessary dependencies to your project.

Configuring OAuth2 client credentials (client-id and client-secret).

Implementing a simple SecurityConfig for OAuth2 login.

  

