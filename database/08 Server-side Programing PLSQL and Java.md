# Server-Side Programing: PL/SQL and Java

## Introduction to Server-Side Programing

* loop 나 If 는 SQL 에서는 사용할 수 없다.
* client/Server-side 프로그램
  * Client Side : C 나 Java 내에 SQL 을 등록. JDBC 나 OCI 로 DB 와 연결
  * Server Side : DB 내에 Data logic 이 존재.
* Servier side 
  * DB 내 코드를 활용하니, 네트워크 트래픽을 안타네.
  * DB 내에 있으니 재사용성이 높다.
  * PL/SQL 과 Java 가 있다.

## Overivew of PL/SQL

* 2가지 카테고를 가진다.
  * Subprogram : procedure, function, package.
  * anonymous block : declare 절

### PL/SQL Subprograms

* 장점
  * 성능이 좋다.
    * 요청하고 결과를 받는 네트워크 처리량이 적다.
    * 이미 DB 내에 Compile 했기 때문에 실행 시간을 단축할 수 있다. 
    * SGA 에 Shared Pool 에 존재하면, DB 는 Disk 에서 호출 할 필요 없이 바로 실행할 수 있다.
  * 메모리 할당
    * SGA 에 한번 로드하면 다른 사용자들도 공유해서 실행할 수 있다.
  * 생산성이 높다.
    * 이건 상황에 따라 다르지만, DB 가 생산성이 좋긴 하지..
  * 일관성
    * DB 프로시져에 문제가 없다면, 타 프로그램이 사용할 수 있어 재사용성이 높다.
  * 권한관리
    * definer's rights procedure :  생성자의 권한으로 프로그램을 실행할 수 있다. 
      * 프로시져 내의 모든 객체에 대한 권한을 받을 필요가 없다.
      * 그러면 호출 프로그램은 해당 Procedure 에 대한 실행 권한만 있으면 원하는 결과를 얻을 수 있다.
    * 호출자의 퀀한으로 프로그램 실행이 가능하다.
      * 호출자의 권한에 따라 프로그램 실행 여부가 달라지낟.
      * HR Manager 는 급여를 변경할 수 있지만, 일반 직원은 급여를 변경할 수 없도록 제한을 둘수도 있다.

### PL/SQL Run

* native or interpreted 실행이 가능.
  * Interpreted : bytecode 형태로 컴파일. run by a portable virtual computer.(무슨 뜻인지 모르겠음)
  * native : OS 에 맞춰 code 를 저장. 저장한 code 는 Oracle DB에 연결.

## Overview of Java in Oracle Database





## Overview of Triggers



