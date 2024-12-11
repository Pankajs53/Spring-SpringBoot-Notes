**Spring Framework: Notes for Beginners**

### Tight Coupling

- Tight coupling occurs when classes are highly dependent on each other.
- Changing one class often requires changes in another.
- This reduces flexibility and makes the code harder to maintain and test.

### Loose Coupling

- Loose coupling minimizes dependencies between classes.
- Changes in one class do not heavily impact others.
- Dependency Injection (DI) is a design pattern that promotes loose coupling.

### Spring Annotations

#### 1. **@Component**

- Marks a class as a Spring-managed component (bean).
- Enables automatic detection and registration by Spring's component scanning.

```java
@Component
public class MyService {
    // code here
}
```

#### 2. **@Autowired**

- Injects a bean into another bean.
- Can be used with constructors, setters, or directly on fields.

```java
@Autowired
private MyRepository myRepository;
```

#### 3. **Where to Scan Beans**

- Spring scans the current package and its sub-packages by default for beans.
- Use `@SpringBootApplication` to enable this behavior.
- To specify a custom scanning path, use `@ComponentScan`.

```java
@ComponentScan(basePackages = "com.example.myapp")
```

#### 4. **Application Context**

- The application context is the Spring container responsible for managing the lifecycle of beans.
- It resolves dependencies, handles bean scopes, and applies configurations.

#### 5. **Is Context a Container?**

- Yes, the application context acts as a container for Spring beans.
- It provides various features like dependency injection, event propagation, and resource management.

#### 6. **@Primary**

```java
@Primary
@Bean
public MyService myPrimaryService() {
    return new MyService();
}
```

#### 7. **@SpringBootApplication**

- A convenience annotation that combines:
  - `@Configuration`: Marks the class as a source of bean definitions.
  - `@EnableAutoConfiguration`: Automatically configures Spring based on the dependencies.
  - `@ComponentScan`: Scans the current package and sub-packages for components.

```java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

#### 8. **@Qualifier**

- Used to resolve ambiguity when multiple beans of the same type exist.
- Works with `@Autowired` to specify the exact bean to inject.

```java
@Autowired
@Qualifier("mySpecificBean")
private MyService myService;
```

### Dependency Injection (DI) Approaches

1. **Constructor Injection**
   - Dependencies are provided through the class constructor.
   - Ensures immutability and is preferred for mandatory dependencies.

```java
@Autowired
public MyService(MyRepository myRepository) {
    this.myRepository = myRepository;
}
```

2. **Setter Injection**
   - Dependencies are provided through setter methods.
   - Useful for optional dependencies.

```java
@Autowired
public void setMyRepository(MyRepository myRepository) {
    this.myRepository = myRepository;
}
```

3. **Field or Property Injection**
   - Dependencies are injected directly into fields.
   - Simple but less testable.

```java
@Autowired
private MyRepository myRepository;
```

### Creating a Bean Programmatically

- You can declare a class as a bean using a method annotated with `@Bean` inside a configuration class.

```java
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

- Note: `@SpringBootApplication` inherently includes `@Configuration`, so you can define beans there as well.

```java
@SpringBootApplication
public class MyApplication {
    @Bean
    public MyService myService() {
        return new MyService();
    }

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

### Bean Scopes in Spring

#### 1. **Singleton**
- Default scope in Spring.
- A single instance of the bean is created and shared across the entire application context.

```java
@Scope("singleton")
@Component
public class MySingletonBean {
    // code here
}
```

#### 2. **Prototype**
- A new instance of the bean is created every time it is requested from the container.

```java
@Scope("prototype")
@Component
public class MyPrototypeBean {
    // code here
}
```

#### 3. **Request**
- A new instance of the bean is created for each HTTP request.
- Scope exists only for the duration of the request.

```java
@Scope(value = WebApplicationContext.SCOPE_REQUEST)
@Component
public class MyRequestBean {
    // code here
}
```

#### 4. **Session**
- A new instance of the bean is created for each HTTP session.
- Scope exists for the duration of the session.

```java
@Scope(value = WebApplicationContext.SCOPE_SESSION)
@Component
public class MySessionBean {
    // code here
}
```

#### 5. **Application**
- A single instance of the bean is shared across the entire servlet context.

```java
@Scope(value = WebApplicationContext.SCOPE_APPLICATION)
@Component
public class MyApplicationBean {
    // Code here
}
```

#### 6. **WebSocket**
- A new instance of the bean is created for each WebSocket session.
- Scope exists for the duration of the session.

```java
@Scope(value = WebApplicationContext.SCOPE_WEBSOCKET)
@Component
public class MyWebSocketBean {
    // Code here
}
```

### Proxy Mode
- Used when a `@Scope` annotation requires Spring to manage the creation of new instances dynamically, especially when dealing with inner beans of different scopes.

#### Example:
If the main class is singleton but contains a prototype-scoped dependency:

```java
@Component
@Scope("singleton")
public class MainClass {

    @Autowired
    private PrototypeClass prototypeClass;

    public void usePrototype() {
        System.out.println(prototypeClass);
    }
}

@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE, proxyMode = ScopedProxyMode.TARGET_CLASS)
@Component
public class PrototypeClass {
    // Code here
}
```

#### Proxy Behavior:
1. **Both Prototype:**
   - Each call creates a new instance of both the main and inner beans.
2. **Main Prototype, Inner Singleton:**
   - The main bean is different each time, but the inner bean remains the same.
3. **Both Singleton:**
   - Both the main and inner beans remain the same instance every time.
4. **Main Singleton, Inner Prototype:**
   - Normally, both act as singleton unless proxy mode is enabled.
   - Use `proxyMode = ScopedProxyMode.TARGET_CLASS` to ensure new instances of the inner bean on each call.

