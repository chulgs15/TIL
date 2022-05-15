# Spring Cloud

* Spring Cloud Netflix Eureka

  * Service Discovery

    * 각각의 마이크로 서비스의 위치를 저장하고 요청에 따라 위치를 제공하는 역할.
      ![image](https://user-images.githubusercontent.com/22446581/122992220-d71bf200-d3e0-11eb-9286-d198d5f0bd98.png)

  * 버젼별 사용 위치

    * https://spring.io/projects/spring-cloud#overview
      * Table 1. Release train Spring Boot compatibility

  
  ```yaml
  server:
    port: 8761
  
  spring:
    application:
      name: discoveryservice
  
  eureka:
    client:
      register-with-eureka: false
      fetch-registry: false
  ```
  
  
  
  * application.yml
  
    * port : 8761
  
    * spring.application.name : discoervyservice
  
      * 고유한 이름.
  
    * eureka.dlient
  
      * register-with-eureka : false
  
      * fetch-registry : false
  
        * 기본적인 값은 true 인데, 자신의 정보를 전화번호부에 등록한다. 
  
          false 로 하면 등록하지 않는다.

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>

```



# Spring Eureka Client

* 목적
  * 같은 서비스를 Port만 별도로 설정해서 여려개를 실행한다.
* 자동으로 random port 로 실행하기
  * server.port : 0
* 그런데 문제는 랜덤 포트로 여러 서비스를 기동하면 eureka 서버에 1개만 등록된다.
  * server.port 까지 포함해서 서비스 이름을 지정하기 때문에 1개만 등록한다.
  * 그래서 서비스의 이름을 Random 포트까지 포함해서 지정해준다.

## userservice application.yml

```yaml
server:
  port: 0

spring:
  application:
    name: user-service

eureka:
  instance:
    instance-id: ${spring.cloud.client.hostname}:${spring.application.instance_id:${random.value}}
  client:
    fetch-registry: true
    register-with-eureka: true
    service-url:
      defaultZone: http://127.0.0.1:8761/eureka

```

## 결과 화면

![image](https://user-images.githubusercontent.com/22446581/124331238-edd4fc80-dbc9-11eb-8d50-d8e853f5997d.png)



내 host에 docker가 있어서 이렇게 나오는 듯.

![image-20210703064354071](C:\Users\shin\AppData\Roaming\Typora\typora-user-images\image-20210703064354071.png)

# Spring API Gateway Service

## API Gateway Service

* 기능
  * 인증 및 권한 부여
  * 서비스 검색 통합
  * 응답 캐싱
  * 정책, 회로 차단기 및 QoS 다시 시도
  * 속도 제한
  * 부하 분산
  * 로깅, 추적, 상관관계
  * 헤더, 쿼리 문자열 및 청구 변환
  * IP 허용 목록에 추가.

## Netflix Ribbon

* Spring Cloud 에서의 msa 간 통신
  * RestTemplate
  * FeignClient
* Ribbon: Client side Load Balancer
  * 서비스 이름으로 호출
  * Health Check
  * Maintenance 상태로 2.4 이후에는 유지보수 하지 않음.

## Netflix Zuul 구현

* 구현
  * First Service
  * Second Service
  * Netflix Zuul 
    * Gateway 역할을 함.
    * Maintenance 상태로 2.4 이후에는 유지보수 하지 않음.
* 분산 전달 여부를 확인
  * first-service 
  * second-service
  * EnableZullProxy
    * zuul.routes
  * ZullFilter
    * 사전/사후 처리를 넣을 수 있다.
* Filter 
  * Zuul Filter
    * Component 로 등록.
    * 사용자의 요청정보를 사전/사후 필터를 적용할 수 있다.
> 사후 필터는 내가 한번 해보자.

## Spring Cloud Gateway

* 종속성 선택

* cloud.gateway.routes

  ```yaml
  server:
    port: 8000
  
  eureka:
    client:
      register-with-eureka: false
      fetch-registry: false
      service-url:
        defaultZone: http://localhost:8761/eureka
  
  
  spring:
    application:
      name: apigateway-service
    cloud:
      gateway:
        routes:
          - id: first-services
            uri: http://localhost:8081/
            predicates:
              - Path=/first-services/**
          - id: second-services
            uri: http://localhost:8082/
            predicates:
              - Path=/second-services/**
  ```

  

  * id : 라우틴 이름
  * uri : 위치 정보를 입력
  * prediates : 요청 조건

* 비동기를 지원하기 때문에 Zuul 서비스를 대체할 수 있다.

* first/second 서비스 모두 동일하게 생성.

## Spring Cloud Gateway - Custom Filter

![image](https://user-images.githubusercontent.com/22446581/125132757-bde4a680-e13f-11eb-8a0f-66fe3ebdbd7b.png)

* AbstractGatewayFilterFactory를 상속받아 구현
* ServerHttpRequest/ServerHttpResponse 객체를 사용.
  * Servlet을 사용하지 않는다.

```java
package com.example.apigatewayservice.config;

import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.gateway.route.RouteLocator;
import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@RequiredArgsConstructor
public class FilterConfig {

    private final CustomFilter customFilter;

    @Bean
    public RouteLocator gatewayRoutes(RouteLocatorBuilder builder) {
        return builder.routes()
                .route(r -> r.path("/first-services/**")
                    .filters(f -> f.addRequestHeader("first-request", "first-request-header")
                            .addResponseHeader("first-response", "first-response-header")
                            .filter(customFilter.apply(new CustomFilter.Config()))
                            )
                    .uri("http://localhost:8081/"))
                .route(r -> r.path("/second-services/**")
                    .filters(f -> f.addRequestHeader("second-request", "second-request-header")
                            .addResponseHeader("second-response", "second-response-header"))
                    .uri("http://localhost:8082/"))
                .build();
    }

}

```

* application.yml
  * 생성한 Filter를 적용.
  * 굳이 추가하지 않음.
* 차후에 사용자 로그인 기능을 넣을 수 있다.

* Custom Filter

  ```java
  package com.example.apigatewayservice;
  
  import lombok.extern.slf4j.Slf4j;
  import org.springframework.cloud.gateway.filter.GatewayFilter;
  import org.springframework.cloud.gateway.filter.factory.AbstractGatewayFilterFactory;
  import org.springframework.http.server.reactive.ServerHttpRequest;
  import org.springframework.http.server.reactive.ServerHttpResponse;
  import org.springframework.stereotype.Component;
  import reactor.core.publisher.Mono;
  
  @Component
  @Slf4j
  public class CustomFilter extends AbstractGatewayFilterFactory<CustomFilter.Config> {
  
      public CustomFilter() {
          super(Config.class);
      }
  
      @Override
      public GatewayFilter apply(Config config) {
          return (exchange,chain) -> {
              ServerHttpRequest request = exchange.getRequest();
              ServerHttpResponse response = exchange.getResponse();
  
              log.info("Custom PRE Filter : Request uri -> {}", request.getId());
  
              return chain.filter(exchange).then(Mono.fromRunnable(() -> {
                  log.info("Custome POST Filter : response code -> {}", response.getStatusCode());
  
              }));
          };
      }
  
      public static class Config{
  
      }
  }
  ```
  
  

### Custom filter 를 RouteLocator 에 추가할 수 없을 까?

* 예제에서는 Custom Filter 를 application.yml에 설정했지만, 이전 gatewayRoutes에 추가할 수 없을까?

* 아래와 같이 filter를 추가하면 가능.

  ```java
      @Bean
      public RouteLocator gatewayRoutes(RouteLocatorBuilder builder) {
          return builder.routes()
                  .route(r -> r.path("/first-services/**")
                          .filters(f -> f.addRequestHeader("first-request", "first-request-header")
                                  .addResponseHeader("first-response", "first-response-header")
                                  .filter(customFilter.apply(new CustomFilter.Config())))
                          .uri("http://localhost:8081"))
              ...
              .build();
      }
  
  ```

### @Configuration과 application.yml filter 중 어떤 Filter를 적용하는가?

* application.yml 의 필터는 CustomFilter 다.
* 확인해본 결과 @Configuration 필터만 적용했다.

## Spring Cloud Gateway - Global Filter

* Custom Filter 와 동일하며, 이전 Config 클래스에 인스턴스 변수를 추가할 수 있다.
* Global 필터와 Custom 필터의 실행 순서
  1. Global 필터 시작
  2. Custom 필터 시작
  3. Custom 필터 종료
  4. Global 필터 종료

#### application.yml

```yaml
server:
  port: 8000

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://localhost:8761/eureka
spring:
  application:
    name: apigateway-service
  cloud:
    gateway:
      default-filters:
        - name: GlobalFilter
          args:
            baseMessage: spring cloud gateway global filter
            preLogger: true
            postLogger: true
      routes:
        - id: first-services
          uri: http://localhost:8081/
          predicates:
            - Path=/first-services/**
          filters:
            - CustomFilter
        - id: second-services
          uri: http://localhost:8082/
          predicates:
            - Path=/second-services/**
          filters:
            - CustomFilter

```

> 코드는 Custom Filter와 동일하지만 yml 파일 사용법이 중요.

## Spring Cloud Gateway - Logging Filter

* 로깅 필터가 따로 있는 것이 아니라, 새로운 필터를 정의해서 어떻게 활용하는가를 알 수 있는 것이 핵심.

### LoggingFilter

* OrderedGatewatFilter의 `(exchage, chain)` 은 GatewayFilter Interface 의 구현체다.

```java
package com.example.apigatewayservice;

import lombok.Data;
import lombok.extern.slf4j.Slf4j;
import org.springframework.cloud.gateway.filter.GatewayFilter;
import org.springframework.cloud.gateway.filter.OrderedGatewayFilter;
import org.springframework.cloud.gateway.filter.factory.AbstractGatewayFilterFactory;
import org.springframework.core.Ordered;
import org.springframework.http.server.reactive.ServerHttpRequest;
import org.springframework.http.server.reactive.ServerHttpResponse;
import org.springframework.stereotype.Component;
import reactor.core.publisher.Mono;

@Component
@Slf4j
public class LoggingFilter extends AbstractGatewayFilterFactory<LoggingFilter.Config> {

    public LoggingFilter() {
        super(Config.class);
    }

    @Override
    public GatewayFilter apply(Config config) {
        GatewayFilter gatewayFilter = new OrderedGatewayFilter((exchange, chain) -> {
            ServerHttpRequest request = exchange.getRequest();
            ServerHttpResponse response = exchange.getResponse();

            log.info("LoggingFilter baseMessage -> {}", config.getBaseMessage());

            if (config.isPostLogger()) {
                log.info("LoggingFilter PRE Filter -> {}", request.getId());
            }

            return chain.filter(exchange).then(Mono.fromRunnable(() -> {
                if (config.isPostLogger()) {
                    log.info("LoggingFilter POST Filter -> {}", response.getStatusCode());
                }
            }));
        }, Ordered.LOWEST_PRECEDENCE);

        return gatewayFilter;
    }

    @Data
    public static class Config {
        private String baseMessage;
        private boolean preLogger;
        private boolean postLogger;
    }
}

```

### application.yml

```yaml
server:
  port: 8000

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://localhost:8761/eureka
spring:
  application:
    name: apigateway-service
  cloud:
    gateway:
      default-filters:
        - name: GlobalFilter
          args:
            baseMessage: spring cloud gateway global filter
            preLogger: true
            postLogger: true
      routes:
        - id: first-services
          uri: http://localhost:8081/
          predicates:
            - Path=/first-services/**
          filters:
            - CustomFilter
        - id: second-services
          uri: http://localhost:8082/
          predicates:
            - Path=/second-services/**
          filters:
            - name: CustomFilter
            - name: LoggingFilter
              args:
                baseMessage: Hi, There
                preLogger: true
                postLogger: true

```

## Spring Cloud Gateway - Load Balancer

![image](https://user-images.githubusercontent.com/22446581/126882196-d975b091-964a-4651-a77f-623580aa5476.png)

* API Gateway 와 eureka 서버의 연결성을 확인한다.
  * 신기한 점은 `lb://`주소를 eureka에서 확인해서 서비스 url로 전달한다는 점이다.

### application.yml (apigateway-servce)

```yaml
server:
  port: 8000

eureka:
  client:
    fetch-registry: true
    register-with-eureka: true
    service-url:
      defaultZone: http://localhost:8761/eureka
spring:
  application:
    name: apigateway-service
  cloud:
    gateway:
      default-filters:
        - name: GlobalFilter
          args:
            baseMessage: spring cloud gateway global filter
            preLogger: true
            postLogger: true
      routes:
        - id: first-services
          uri: lb://MY-FIRST-SERVICE
          predicates:
            - Path=/first-services/**
          filters:
            - CustomFilter
        - id: second-services
          uri: lb://MY-SECOND-SERVICE
          predicates:
            - Path=/second-services/**
          filters:
            - name: CustomFilter
            - name: LoggingFilter
              args:
                baseMessage: Hi, There
                preLogger: true
                postLogger: true
```

## First-Services Random port

first-services 에 random port를 적용해서 서비스를 분산시킨다.
```yaml
server:
  port: 0

spring:
  application:
    name: user-service

eureka:
  instance:
    instance-id: ${spring.cloud.client.hostname}:${spring.application.instance_id:${random.value}}
  client:
    fetch-registry: true
    register-with-eureka: true
    service-url:
      defaultZone: http://127.0.0.1:8761/eureka
```
## 서버 포트 확인.
`check` entry point를 만들어 실행하는 server port를 확인한다. server port를 확인할 수 있는 방법은 아래와 같다.

1. Environmet
2. HttpServletRequest

```java
package com.example.firstservice;

import lombok.extern.slf4j.Slf4j;
import org.springframework.core.env.Environment;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.servlet.http.HttpServletRequest;

@RestController
@RequestMapping("/first-services")
@Slf4j
public class FirstRestController {

    private final Environment environment;

    public FirstRestController(Environment environment) {
        this.environment = environment;
    }

    @GetMapping(value = {"/welcome"})
    public String welcome() {
        return "Welcome to the first service.";
    }

    @GetMapping(value = {"/message"})
    public String message(@RequestHeader("first-request") String header) {
        log.info(header);
        return "Hello World in First Service";
    }

    @GetMapping(value = {"/check"})
    public String check(HttpServletRequest request) {
        log.info("HttpServletRequest server port is " + request.getServerPort());
        String message = "Environment server port is " + environment.getProperty("local.server.port");
        log.info(message);
        return message;
    }
}
```

# E-commerce 애플리케이션.

| 구성요소           | 설명                                           |
| ------------------ | ---------------------------------------------- |
| Git Repository     | 소스 관리                                      |
| Config Server      | Git 저장소에 등록된 프로파일 정보 및 설정 정보 |
| Eureka Server      | 마이크로 서비스 등록 및 검색                   |
| API Gateway Server | 마이크로서비스 부하 분산 및 서비스 라우팅      |
| Microservices      | 회원 MS, 주문 MS, 상품 MS                      |
| Queueing System    | 마이크로서비스 간 메시지 발행 및 구독          |

## Users Microservice

* 이전 Hostname 을 사용했던 것과 달리, application 이름을 eureka 에 등록할 수 있다.

```yaml
eureka:
  instance:
    instance-id: ${spring.application.name}:${spring.application.instance_id:${random.value}}
```

* Environment 객체를 사용하면 application.yml 파일에 등록한 값을 가져올 수 있다.

```java
@RestController
@RequestMapping("/")
public class UserController {

    private Greeting greeting;

    private Environment environment;

    public UserController(Greeting greeting, Environment environment) {
        this.greeting = greeting;
        this.environment = environment;
    }

    @GetMapping("health_check")
    public String status() {
        return "It is working : user-service";
    }


    @GetMapping("/welcome")
    public String welcome() {
        return greeting.getMessage();
    }

}
```















