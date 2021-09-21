# Introduction to Oracle Database

1. About Relational Databases
2. Schema Objects
3. Data Access
4. Transaction Management
5. Oracle Database Architecture
6. Oracle Database Documentation Road Map

# Oracle Database Architecture

* database server is the key to information management

## Database and Insatance

  * DB는 적어도 1개 이상의 인스턴스로 구성한다.
  * 정의를 구분하자면...
    * Database
      * DB는 데이터를 저장하기 위한 파일의 집합.  
      * Instance 와 독립적으로 존재.
    * Instance
      * DB 파일을 관리하기 위한 메모리 구조 집합체.
      * SGA 와 백그라운드 프로세스로 구성.
      * Database 파일과 독립적으로 존재.
  * 그림 : client process - pga - server process 
    - [x] 이건 어떻게 확인할 수 있을까?

### Multi-tenant Architecture
  * Multi-tenant Container Database (CDB)
    * 0...N개의 Pluggable Database 를 가진 하나의 물리적인  DB.
  * Pluguable Database  
    * 스키마, 스키마 오브젝트 등을 가진 DB
  * 장점
    * 여러 물리적인 서버를 1대의 서버로 통합해서 생긴 장점?
    * 비용절감, PDB와 CDB 관리자 분리?

### Database Consolidation
  * 데이터 이동.  non-cdb => cdb
  * 예시를 보여주네.
    * Non-CDBs
      * 각각의 시스템이 파일과 메모리를 별도로 점유하고 있다.
    * CDB
      * 물리적으로 MYCDB가 Oracle Database. MYCDB가 하나의 데이터 베이스 인스턴스임.
      * CDB Root 로 접속하면 PDB 들이 가진 스키마와 Object 를 가질 수 있다. 

### Application Containers
  * Application Model : Application root 에 저장한 데이터와 메타데이터의 집합. Versioned And Named.
  * 예를 들면, Application Model 은 Object 들의 집합.
  * [x] 뭔지 잘 모르겠음

### Sharding Architecture
  * 여러 Oracle DB 사이로 수평적으로 데이터를 자르는 것이다.
  * 전제는 다른 DB 에 같은 테이블 구조로 데이터가 분리되어 있어야 하네.
  * Sharding director 가 요청에 따라 검색할 DB를 정리함.

## Database Storage Structure
  * Oracle Database Processes
    * Physical Storage Structures
      * 데이터를 저장하는 실제 구조
        1. Data files : 데이터 저장
        2. Control Files : 메타데이터(DB 이름, 위치.)
        3. Online redo log files : DB 데이터 파일과 Control File 의 변화를 저장.
    * Logical Storage Structures
      1. Data blocks : 가장 작은 단위
      2. Extents : 연속되는 데이터 블럭.
      3. Segments : set of extents
      4. Tablespaces : DB 는 여러 Tablespaces 들의 집합. Segments들의 logical container. Tablespaces 는 하나 이상의 데이터 파일을 가진다.

## Database Instance Structures

### Oracle Database Processes
  1. Client Processes : 요청하는 프로세스
  2. Background Process : 운영과 유지를 위한 프로세스(?)
  3. Server Processes : Client Process 와 통신하면 요청을 처리하는 프로세스.
  * Oracle Process = 2 +3
###  Instance Memory Structures
  1. SGA : 오라클이 데이터를  CRUD 하기 위한 공간.
  2. PGA : Oracle DB 에 접속하는 User 에게 할당하는 메모리
#### Application and Networking Architecture

#####  Application Architecture
  1. Client - Server Architecture
  2. Multi tier Architecture
  3. Simple Oracle Document Access(SODA)

#####  Oracle Net Services Architecture

  * Oracle Net Services : DB와  Network Protocol 사이의 Interface 를 담당.
    * Oracle Net : Client 와 DB Server 사이에 network session을 생성. 
        * Oracle Net Listener : Listener 를 통해 연결이 설정되면 Client 와 Server 가 직접 통신한다.
    
      1. Dedicated Server Architecture : Client process 와 Server Process 가 1:1 로 진행.
      2. Shared Server Architecture : multiple sessions.

# 참조

* https://1duffy.tistory.com/18
  * SGA/PGA
