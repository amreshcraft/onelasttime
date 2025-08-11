
## **Introduction to Spring Boot**

- **What it is:**  
    Spring Boot is a **framework on top of Spring** that removes most manual configuration and allows you to quickly build production-ready applications.
    
- **Why it exists:**  
    Traditional Spring requires lots of XML or Java config (DataSource, DispatcherServlet, etc.). Spring Boot removes that boilerplate by using:
    
    - **Convention over configuration**
        
    - **Auto-configuration**
        
    - **Embedded servers** (Tomcat, Jetty, Undertow)
        
- **Goal:**  
    Make Spring development **fast, opinionated, and production-ready**.
    

---

## **Key Features**

1. **Auto-Configuration**
    
    - Uses `@EnableAutoConfiguration` (inside `@SpringBootApplication`).
        
    - Spring Boot scans the classpath and configures beans automatically based on dependencies present.
        
    - Example: If `spring-boot-starter-data-jpa` is in classpath, it configures Hibernate, EntityManager, and TransactionManager.
        
2. **Embedded Server**
    
    - No need to deploy WAR to external Tomcat.  
        Run `java -jar app.jar` — Spring Boot starts its own Tomcat/Jetty.
        
3. **Starter Dependencies**
    
    - Predefined dependency sets like `spring-boot-starter-web`, `spring-boot-starter-security`.
        
    - Removes version conflict headaches.
        
4. **Production-ready Features**
    
    - Actuator endpoints: `/actuator/health`, `/actuator/metrics`
        
5. **Externalized Configuration**
    
    - `application.properties` / `application.yml` or environment variables.
        
    - Priority order ensures flexibility.
        
6. **Spring Boot CLI**
    
    - For Groovy-based fast prototyping.
        

---

## **Spring Boot Core Annotations**

- **`@SpringBootApplication`** =  
    `@Configuration` + `@EnableAutoConfiguration` + `@ComponentScan`
    
- **`@RestController`** =  
    `@Controller` + `@ResponseBody`
    
- **`@Bean`** – Explicitly define a bean in the Spring Context.
    
- **`@Component`, `@Service`, `@Repository`** – Component scanning stereotype annotations.
    
- **`@ConfigurationProperties`** – Map properties file values into POJOs.
    
- **`@Value`** – Inject property value directly.
    
- **`@ConditionalOnClass`, `@ConditionalOnMissingBean`** – Used heavily in auto-configuration.
    

---

## ** Application Structure (Best Practice)**

```
src/main/java
  └── com.example.project
      ├── ProjectApplication.java  # Main class
      ├── controller               # REST endpoints
      ├── service                  # Business logic
      ├── repository               # Data access (Spring Data JPA)
      ├── model/entity             # Entities/DTOs
      ├── config                   # Config classes
```

- **Why this matters:** Keeps code clean, easier testing, clear separation of concerns.
    

---

## **Dependency Injection (DI) in Spring Boot**

- **How it works:**
    
    - Spring container manages object creation and wiring.
        
    - Beans are instantiated and injected automatically based on:
        
        - `@Autowired` (by type)
            
        - Constructor injection (preferred in production)
            
- **Example:**
    

```java
@Service
public class UserService {
    private final UserRepository repo;

    public UserService(UserRepository repo) {
        this.repo = repo; // Constructor injection
    }
}
```

- **Best practice:**  
    Always use **constructor injection** for immutability & easier testing.
    

---

## **Auto-Configuration Internals**

- When app starts:
    
    1. `@EnableAutoConfiguration` triggers `SpringFactoriesLoader`.
        
    2. Reads `/META-INF/spring.factories` from all jars in classpath.
        
    3. Loads `@Configuration` classes conditionally (`@ConditionalOnClass`, `@ConditionalOnMissingBean`).
        
    4. Registers beans into the application context.
        
- **Debugging tip:**  
    Use `--debug` flag to see which auto-configurations are applied.
    

---

## **Configuration & Profiles**

- `application.properties` or `application.yml`
    
- Multiple environments:
    
    - `application-dev.properties`
        
    - `application-prod.properties`
        
- Activate with:
    

```bash
java -jar app.jar --spring.profiles.active=dev
```

- **Priority order (high → low)**:
    
    1. Command line args
        
    2. `application-{profile}.properties`
        
    3. `application.properties`
        
    4. Default configs inside jars
        

---

## ** Running & Packaging**

- **Run inside IDE** – Right-click main class → Run.
    
- **Run via Maven/Gradle**:
    

```bash
mvn spring-boot:run
```

- **Package as executable JAR**:
    

```bash
mvn clean package
java -jar target/app.jar
```

---

## ** Embedded Server Details**

- **Default:** Tomcat on port 8080
    
- Change port:
    

```properties
server.port=9090
```

- **Switch server:** Add dependency for Jetty, remove Tomcat.
    
- Spring Boot **registers DispatcherServlet automatically**.
    

---

## ** Actuator for Monitoring**

- Add dependency:
    

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

- Access endpoints:
    
    - `/actuator/health`
        
    - `/actuator/info`
        
    - `/actuator/metrics`
        
- Secure via Spring Security in production.
    

---

## ** Logging**

- Uses **Logback** by default.
    
- Change logging level in properties:
    

```properties
logging.level.org.springframework=DEBUG
logging.file.name=app.log
```

- Supports SLF4J abstraction → can plug Log4j2 if needed.
    

---

## **Exception Handling**

- **Controller-level**: `@ExceptionHandler`
    
- **Global**: `@ControllerAdvice`
    
- **Example**:
    

```java
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handle(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                             .body(ex.getMessage());
    }
}
```

---

## ** Database Integration**

- Add `spring-boot-starter-data-jpa` & DB driver.
    
- Config in `application.properties`:
    

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/db
spring.datasource.username=postgres
spring.datasource.password=pass
spring.jpa.hibernate.ddl-auto=update
```

- Repository interface:
    

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {}
```

---

## ** Testing**

- **Unit Tests** with `@SpringBootTest` and `@MockBean`
    
- Example:
    

```java
@SpringBootTest
public class UserServiceTest {
    @MockBean
    private UserRepository repo;

    @Autowired
    private UserService service;

    @Test
    void testGetUser() {
        when(repo.findById(1L)).thenReturn(Optional.of(new User()));
        assertNotNull(service.getUser(1L));
    }
}
```

---

## ** Best Practices**

- Use constructor injection over `@Autowired` field injection.
    
- Keep controllers thin, services thick.
    
- Use DTOs for request/response (avoid exposing entities directly).
    
- Enable Actuator in prod but secure it.
    
- Handle exceptions globally.
    
- Avoid putting all logic in `main` package — follow layered architecture.
    
