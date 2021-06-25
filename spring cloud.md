# Spring Cloud

* Spring Cloud Netflix Eureka

  * Service Discovery

    * 각각의 마이크로 서비스의 위치를 저장하고 요청에 따라 위치를 제공하는 역할.
      ![image](https://user-images.githubusercontent.com/22446581/122992220-d71bf200-d3e0-11eb-9286-d198d5f0bd98.png)

  * 버젼별 사용 위치

    * https://spring.io/projects/spring-cloud#overview
      * Table 1. Release train Spring Boot compatibility

  * application.yml

    * port : 8761

    * spring.application.name : discoervyservice

      * 고유한 이름.

    * eureka.dlient

      * register-with-eureka : false

      * fetch-registry : false

        * 기본적인 값은 true 인데, 자신의 정보를 전화번호부에 등록한다. 

          false 로 하면 등록하지 않는다.