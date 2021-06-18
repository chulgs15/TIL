

- 내가 이야기 하려는 주제는 무엇인가?

  - Docker 란 무엇인가?
  - Cloud로 운영하는 방식 소개

    - Dev/Ops 와 연결해서 설명해보는 건 어떨까?
  
- 기존의 개발문화에 대한 설명

  ![이미지](https://www.hanumoka.net/images/20180630-weakness-of-waterfall-model_1.png)

  - 기존 워터폴 방식을 설명하면서 문제점을 설명한다.
    - 우리는 기존 워터폴 방삭으로 개발을 진행했습니다. 요구사항 수립, 설계, 구현, 테스트, 이관 및 운영. 이런 5가지 단계로 서비스를 구현합니다.
    - 이 문제점은 현재 진행하는 단계가 완성되지 않으면 다음 단계로 진행하지 못하며, 중간에 요구사항 변경을 받아드리기 어렵다는 점이 있습니다.

- 정의

  ![이미지](https://miro.medium.com/max/728/0*H5SUvwGviMsyMICt.png)

    - 출처: <https://aws.amazon.com/ko/devops/what-is-devops/> 
  - 서비스를 빠른 속도로 제공할 수 있도록 조직의 역량을 향상시키는 문화 철학, 방식 및 도구의 조합입니다.
  - DevOps 모델에서는 개발팀과 운영팀이 단일팀으로 병합되어 엔지니어가 개발에서 테스트, 배포, 운영까지 전체를 관리합니다.
  - 기존 워터폴의 단점인 모든 단계 Step by     Step 으로 확인과 요구사항 확장의 불가능을 저 흐름에 맞게 

- 단계

  - 단계에 대한 설명과 예시가 필요하네.

    - 만약 쇼핑몰 서비스를 구축한다고 생각해볼까요? 어떤 서비스를 만들지 설계하고, 설계대로 코드를 작성합니다. 작성한 코드를 빌드해서 문법문제가 없는지 확인하고 Test 를 진행합니다. Test에 성공한 빌드는 운영환경에 전달하는       Release/Deploy를 진행합니다. 자, 이제 운영자는 모니터링을 하면서 이슈나 문제점을 찾습니다. 
    - 이슈와 문제점을 개선하기 위해 새로운 설계를 진행하며 다시 Dev/Ops  프로세스를 시작합니다.

  - DevOps 방식을 사용하여 속도가 느리고 수동으로 수행되던 프로세스를 자동화

    - 여러 단계가 있지만 자동화 영역은 뭐가 있을까요
      - BUILD, TEST,  RELEASE, DEPLOY, MONITOR

    - 반대로 비자동화 영역은

      - PLAN, OPREATE,  CODE

  - 이중 Docker는 BUILD, DEPLOY를 담당합니다.

    ![이미지](https://blog.kakaocdn.net/dn/btzNcP/btqAXhTnbc7/2RXkMqFM9QY7nkix6LSJrK/img.png)

    - 그림 필요.

- Docker

  - 정의
    - 

  - 컨테이너란?

    - 컨테이너 > 이미지 
    - Layer

  - 최근에 확인한 Oracle19c 이슈는 이렇게 찾았음.

  - 장점

    - 환경 구성이 필요없다.

  - 단점

    - 도커의 단점이 뭐지??

- 내가 보여줄 프로그램



 

- 아키텍쳐 + AWS     Beanstalk + RDS

  - 위 그림에 맞는 내 프로그램이 필요하네.

- Frontend

- Backend

- AWS Beanstalk

- RDS

- 시연

  - 새로운 서비스를 만들 필요가 있음
  - Frontend 수정
  - Backend 수정
  - docker compose 
  - Github push
  - Travis CI
  - AWS Beanstalk 진행.
  - 결과 확인.

- 참고

  - https://brunch.co.kr/@fits-b/2
    - DEVOPS 의 단점?
  - https://www.44bits.io/ko/post/why-should-i-use-docker-container
    - 눈송이 서버
  - https://cloud.kt.com/portal/user-guide/education-eduadvanced-edu_adv_2
    - layer

 

 

* 문화
* 개발
* 시현

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 