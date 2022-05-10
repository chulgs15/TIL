# Data Dictionay and Dynamic Performance views

## Overview of the Data Dictionary

* DB 의 메타데이터, Object, User, 권한 등의 정보를 가진 테이블

* DDL 을 통해서 변경이 가능하다.

* `DBA_`, `ALL_`, `USERS_` 단위로 데이터를 가져갈 수 있다.
  
  * `USER`: 특정 계정 스키마 정보
  
  * `ALL_` : 자기 소유 또는 권한 부여받은 객체만 접근 가능.
  
  * `DBA_` : 전체 정보

* System 테이블 스페이스에 저장한다.

* Data Dictionary에 있는 정보는 Data Dictionary Cache 에 보관한다.
  
  * Commnet 는 DB Buffer Cache 에 보관한다.

## Overview of the Dynamic Performace Views

* DB 활동에 관련 정보를 가진다. 예를 들미녀
1. System and sesion parameters

2. Memory usage and allocation

3. File States

4. SQL execution

5. Statistics and metrics
* Fixed Views 라고 부른다.
  
  * `GV$` 도 있는데, RAC 환경에서 사용.

* `catalog.sql` 스크립트로 Fixed Views 생성.

* DB 상태에 따라 조회할 수 있는 View 가 다르다.
  
  * DB 가 NO MOUNT 될 때, `V$INSTANCE` 와  `V$BGPROCESS` 를 조회할 수 있다.
  
  * MOUNT 될 때, `V$DATAFILE`을 조회할 수 있다.

## Database Object Metadata

* `DBMS_METADATA` 패키지 설명.
