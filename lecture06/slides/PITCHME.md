---
<!-- .slide: class="center" -->
@title[Kotlin]

### Lecture 6
#### Spring


---
@title[Sign in]
<!-- .slide: class="center" -->

Please [**sign in**](https://sphere.mail.ru/)
 
*For off-line students only*


---
@title[Get ready]
<!-- .slide: class="center" -->
```bash
git fetch upstream
git checkout -b lecture06 upstream/lecture06
cd lecture06
```

open as new project


---
@title[Agenda]
1. @css[highlight](Annotations)
1. Spring, Spring Boot
1. Inversion of Control, Dependency Injection
1. Beans, ApplicationContext
1. Practice

---
@title[Annotations demo]

---

@title[Agenda]
1. Annotations
1. @css[highlight](Spring, Spring Boot)
1. Inversion of Control, Dependency Injection
1. Beans, ApplicationContext
1. Practice

---
@title[Overall picture of our chat]
**Spring boot** starts application  
that starts embedded **tomcat**  
that runs **spring**  
that runs our logic  
  
Let's discuss how @css[highlight](spring) runs our logic

---
@title[Spring]
<img src="lecture06/slides/assets/images/spring-by-pivotal.png" alt="spring" align="center"/>  
is a universal open-source framework, used to develop web applications  
https://spring.io/  
  
First version - **2002**

---
@title[Spring modules]
It includes a number of modules for different functionality:
- Spring MVC for building Web Applications
- Working with Databases
- Messaging
- RPC
- Security
- Testing
  
Today we will build web application with @css[highlight](spring boot web)

---
@title[Spring Boot]
Spring is a powerful tool and has a lot of configuration options.  
**Spring Boot** is a project, that makes working with Spring easier:
- embedded tomcat included with servlet container
- minimum configuration, sane defaults
- metrics, health checks and externalized configuration
https://projects.spring.io/spring-boot/  
  
First version: **2014**
  
**With Spring Boot our life is much easier**


---
@title[Spring boot distribution]
Plugins necessary to build spring-boot application in kotlin:
```kotlin
plugins {
    // ...
    // all kotlin object members open by default for beans
    id("org.jetbrains.kotlin.plugin.spring") version ktVersion
    // can build spring boot application with gradle
    id("org.springframework.boot") version "2.0.5.RELEASE"
    id("io.spring.dependency-management") version "1.0.5.RELEASE"
}
```

Spring boot dependencies:
```kotlin
// main web module
compile("org.springframework.boot:spring-boot-starter-web")
// monitoring endpoint
compile("org.springframework.boot:spring-boot-starter-actuator")
```
---

@title[Spring boot actuator]
Spring boot actuator - useful dependency, providing web interface to meta data of application and even interact with it  
  
[Actuator endpoints](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-endpoints.html)

By default most endpoints are disabled. To enable them we need to enable them in **application.properties**

---

@title[application.properties]
The standard way to configure kotlin application - **application.properties** should appear in classpath  
To enable actuator endpoints:
```properties
management.endpoints.web.exposure.include=*
```
We also can configure actuator and server ports there:
```properties
#server port:
server.port = 8080
#actuator port:
management.server.port = 7001
```

---
@title[Useful actuator endpoints]
@css[highlight](/actuator/health)  
overall application status  
  
@css[highlight](/actuator/mappings**)  
available mappings  
  
@css[highlight](/actuator/beans)  
all beans in context
---

@title[Agenda]
1. Annotations)
1. Spring, Spring Boot
1. @css[highlight](Inversion of Control, Dependency Injection)
1. Beans, ApplicationContext
1. Practice

---

@title[Inversion of Control (IoC)]
**Principle:** control flow is transferred to external framework  
**Why:** loose coupling, easier to develop, easier to test

---

@title[Dependency Injection]
**DI** is an implementation of **IoC** principle
Objects lifecycle is managed by external framework (**IoC container**)
- instantiation
- wiring

---

@title[Spring provides IoC container]
[Beans are managed with container](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans)  
Interface of **IoC Container** in Spring:  
**org.springframework.context.ApplicationContext**  
- methods for accessing application components. **ListableBeanFactory**
- methods to load file resources in a generic fashion. **ResourceLoader**
- methods to publish events to registered listeners. **ApplicationEventPublisher**
- methods to resolve messages, supporting internationalization. **MessageSource**

---

@title[Agenda]
1. Annotations
1. Spring, Spring Boot
1. Inversion of Control, Dependency Injection)
1. @css[highlight](Beans, ApplicationContext)
1. Practice

---

@title[Beans]
[What is bean?](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-definition)  
@css[highlight](Beans) are kotlin/java objects, that are managed by **IoC Container**  
  
How to make **bean** out of **POJO** (Plain Old Java Object)?  
With bean definition configuration

---
@title[Spring configuration]
There are several options for beans configuration:
- XML Description
- Groovy Description
- Annotations
  
We will use annotations as this is the cleanest one

---

@title[Beans Detection]
For spring to create and manage beans, we must provide bean definitions  
**How to create bean definition with annotations:**
- mark class with **@Configuration**/**@Component**/**@Controller**/**@Service**/**@Repository** or annotations, inheriting their semantics
- mark any method inside such class with **@Bean** (config method)

---

@title[Beans autowiring]
https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-autowired-annotation  
Once we have beans definitions, we can inject those beans with **@Autowired**  
Possible targets:
- constructor
- field
- setter method
- config method

---

@title[ByType and ByName autowiring]
```kotlin
@Controller
@RequestMapping("/example")
class ExampleController {
    @Autowired // How to know which bean to autowire
    lateinit var beanRegistry: BeanRegistryService
}
```
- ByType: it will search the bean with type **ConnectionQueue** or implementation in ApplicationContext
- ByName: with **@Qualifier** annotation we can autowire bean by name  
https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-autowired-annotation-qualifiers
---

@title[Bean scopes]
Beans can have different life span depending on requirements.  

[Bean scopes in docs](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes)
    
## Common scopes  
- singleton (default)
- prototype: single bean definition to any number of object instances
- request: single bean definition to the lifecycle of a single HTTP request
- websocket: single bean definition to the lifecycle of a WebSocket
...

---

@title[Spring: see documentation]
Both basic concepts and details are fully covered in spring [documentation](https://docs.spring.io/spring/docs/current/spring-framework-reference/index.html).


---
@title[Match-maker practice]
<img src="lecture06/slides/assets/images/mm.png" alt="spring" align="center"/>  

---

@title[Match-maker practice]
https://github.com/rybalkinsd/kotlin-boot-camp/tree/lecture06/assignments/05


---
@title[That's all for today]

Please leave your @css[highlight](feedback).
 
*Offline students will get the link via email*
