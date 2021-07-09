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
  * id : 라우틴 이름
  * uri : 위치 정보를 입력
  * prediates : 요청 조건
* 비동기를 지원하기 때문에 Zuul 서비스를 대체할 수 있다.
* first/second 서비스 모두 동일하게 생성.











































