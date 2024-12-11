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
    // logic here
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
- Marks a bean as the default choice when multiple beans of the same type exist.
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

