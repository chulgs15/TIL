Part1 Oracle Relational Data Structures

1. Tables and Table Cluster
2. Indexes and Index-Organized Tables.
3. Partitions, Views, and Other Schema Objects
4. Data Integrity
5. Data Dictionary and Dynamic Performance Views

# Tables and Table Clusters

* Introduction to Schema Objects

  * Schema : 자료 구조의 Container.

  * DB User 는 1개의 Schema 를 가진다. 

  * Schema Object Types

    * Table, Index, Partition, views, Sequences, Dimensions, Synonyms, PL/SQL 

      > :blue_book:Dimensions 가 뭐지??

  * Schema Object Storage
    * Table 과 Index 는 segment 에 저장한다. 
    * View 나 Sequence 는 메타 데이터만 가지고 있다.
    * Oracle DB는 schema object 를 tablespace에 저장한다. 
      * schema 와 tablespace 사이에는 연관성이 없다. 즉,  tablespace 에는 어떤 schema 의 데이터도 들어올 수 있다.
  * Schema Object Dependencies
    * 타 객체를 참조하면 종속성이 생기지. 참조하는 객체의 최신성도 확인.
    * 종속성에 문제가 생기면 객체의 상태를 invalid 로 바꾼다.
    * 예제는 테이블의 컬럼 타입을 변경하면서 테이블을 참조한 procedure 가 Invalid가 된다.
      * 다시 Run or Recompile 시 valid 가 된다.  
  * Sys and System schemas
    * https://coder9084.tistory.com/155
    * sys 는 data dictionary 테이블/뷰의 데이터를 저장. DB 생성과 삭제가 가능하다.
    * system 은 DB 생성과 삭제는 불가능하다. 운영, 관리를 위한 관리자 계정이다.
  * Sample Schemas
    * 이런게 있다. 

* Overview of Tables

  * 카테고리로 아래와 같이 분류할 수 있다.
    1. Relational Tables
    2. Object Tables
  * 조직하는 성격에 따라서 아래와 같이 바꿀 수 있다. 
    1. heap-organized table : `Create Table`  로 만드는 테이블
    2. index-organized table : primary key로 엮긴 테이블.
    3. external table : read only Table.
  * permanent or temporary 로 구분.
  * Columns
    * 내가 아는 그 컬럼이네.
    * Virtual Columns : 연산으로 발생하는 컬럼. 공간 소요 X
    * Invisible Columns : `SELECT *` 로 나타나지 않은 컬럼. 직접 이름을 지정해야 데이터를 볼 수 있다.  12c 부터 지원하는 기능.
  * Rows
  * Example : Create Table and Alter table statements
  * Oracle Data Types
    * Character Data Types
      * https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=darkturtle&logNo=50042229931
      * Character / Byte 로 길이를 설정할 수 있다. 
      * NLS_LENGTH_SEMANTICS : 설정 파라미터
      * VARCHAR2 and CHAR Data Types
        * 가변길이와 고정길이 저장방식의 차이
      * NCHAR and NVARCHAR2 Data Types
        * Unicode 기반의 문자열 저장 방식
        * Character Set  = AL16UTF16  or UTF8
    * Numeric Data Types
      * Fixed Point Numbers
        * 고정형 숫자 표기. Precision, Scale 을 설정.
      * Floating Point Numbers
        * BINARY_FLOAT, BINARY_DOUBLE
        * 근사치를 저장하기 때문에 정확한 표현이 어렵다.
    * Datetime Data Types
      * DATE Data Type 
    * Rowid Data Types
    * Format Models and Data Types
  * Integrity Constraints
  * Table Storage
  * Table Compression

* Overview of Table Clusters

* Overview of Attribute-Clustered Tables

* Overview of Temporary Tables.

* Overview of Object Tables.
  
  * 



