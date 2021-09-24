Part1 Oracle Relational Data Structures

1. Tables and Table Cluster
2. Indexes and Index-Organized Tables.
3. Partitions, Views, and Other Schema Objects
4. Data Integrity
5. Data Dictionary and Dynamic Performance Views

# Tables and Table Clusters

## Introduction to Schema Objects

  * Schema : 자료 구조의 Container.

  * DB User 는 1개의 Schema 를 가진다. 

### Schema Object Types

* Table, Index, Partition, views, Sequences, Dimensions, Synonyms, PL/SQL 
  
> :blue_book:Dimensions 가 뭐지??

### Schema Object Storage

* Table 과 Index 는 segment 에 저장한다. 
* View 나 Sequence 는 메타 데이터만 가지고 있다.
* Oracle DB는 schema object 를 tablespace에 저장한다. 
  * schema 와 tablespace 사이에는 연관성이 없다. 즉,  tablespace 에는 어떤 schema 의 데이터도 들어올 수 있다.

#### Schema Object Dependencies

* 타 객체를 참조하면 종속성이 생기지. 참조하는 객체의 최신성도 확인.
* 종속성에 문제가 생기면 객체의 상태를 invalid 로 바꾼다.
* 예제는 테이블의 컬럼 타입을 변경하면서 테이블을 참조한 procedure 가 Invalid가 된다.
  * 다시 Run or Recompile 시 valid 가 된다.  

##### Sys and System schemas

* https://coder9084.tistory.com/155
* sys 는 data dictionary 테이블/뷰의 데이터를 저장. DB 생성과 삭제가 가능하다.
* system 은 DB 생성과 삭제는 불가능하다. 운영, 관리를 위한 관리자 계정이다.

  * Sample Schemas
    * 이런게 있다. 

## Overview of Tables
* 카테고리로 아래와 같이 분류할 수 있다.
    1. Relational Tables
    2. Object Tables
  * 조직하는 성격에 따라서 아래와 같이 바꿀 수 있다. 
    1. heap-organized table : `Create Table`  로 만드는 테이블
    2. index-organized table : primary key로 엮긴 테이블.
    3. external table : read only Table.
  * permanent or temporary 로 구분.
###  Columns

* 내가 아는 그 컬럼이네.
  * Virtual Columns : 연산으로 발생하는 컬럼. 공간 소요 X
  * Invisible Columns : `SELECT *` 로 나타나지 않은 컬럼. 직접 이름을 지정해야 데이터를 볼 수 있다.  12c 부터 지원하는 기능.

### Rows
* Example : Create Table and Alter table statements

#### Oracle Data Types

#### Character Data Types

* https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=darkturtle&logNo=50042229931
* Character / Byte 로 길이를 설정할 수 있다. 
* NLS_LENGTH_SEMANTICS : 설정 파라미터
* VARCHAR2 and CHAR Data Types
  * 가변길이와 고정길이 저장방식의 차이
* NCHAR and NVARCHAR2 Data Types
  * Unicode 기반의 문자열 저장 방식

  * Character Set  = AL16UTF16  or UTF8

#### Numeric Data Types

##### Fixed Point Numbers

    * 고정형 숫자 표기. Precision, Scale 을 설정.
    * Floating Point Numbers
    * BINARY_FLOAT, BINARY_DOUBLE
    * 근사치를 저장하기 때문에 정확한 표현이 어렵다.

##### Datetime Data Types

* DATE Data Type 
* 내부적으로 숫자로 저장한다. 7 byte 로 저장한다.
* Date Format Model로 표현한다. 기본적으로 (dd-mon-rr)
* 01-JAN-11 의 RR과 YY 의 차이점
 * RR 은 2011 년
 * YY 는 1911 년.
* TIMESTAMP Data Type
* Date data type 의 확장판
* DATETIME 은 `TIMESTAMP WITH TIME ZONE` 과 `TIMESTAMP WITH LOCAL TIME ZONE` 은 데이터를 조회하는 Session에 따라 달라진다.

* Rowid Data Types
 * row 의 주소 정보.
   1. Physical Row ID : heap 테이블, 테이블 cluster, table, index 의 Row 주소.
   2. Logical Row ID : Index Organized Table 의 Row 주소.
   3. Foreign rowid : 오라클은 안쓰는데 그런게 있다.
 * Use of Rowids.
   * B-tree Index 에서 사용. 
   * RowID 타입으로 테이블을 생성할 수 있다.
 * ROWID Pseudocolumn
   * 모든 테이블은 ROWID 와 같은 수도 컬럼을 가진다.

### Format Models and Data Types

* to_char 를 통한 테이터를 문자열을 변경할 수 있다.

### Integrity Constraints

* 제약 조건 
* data dictionary 에 저장

### Table Storage

* Oracle Database 는 테이블 스페이스에 있는 data segment 로 저장한다.

### Table Compression

* row 를 저장할 때, 사용자가 위치를 지정하는 것이 아니다.
    * iot 는 예외임.
* 사용자가 Insert Row 를 진행할 때, 처음 찾은 segment에서 비어있는 공간에 입력한다. 순서를 보장하지 않음.
* 데이터의 컬럼의 순서는 보장하지만 아래와 같은 예외 사항은 있다. 
    1. Long 타입의 컬럼을 저장할 때,
    2. 새로운 Column 을 추가했을 때.

### Row Storage

* db 는 데이터 Block 안에 row 를 저장한다.
* 가능한만큼 각각의 Row 를 row piece 에 저장하려고 한다.
    * 하지만 크기가 너무 크면 Multi Row piece 에 저장한다. 
* 이건 잘 모르겠지만 Table Cluster 의 row 는 뭔가 다른가 보다.

### Rowids of Row Pieces
* rowid 는 10-bytes 물리적인 주소다.
* 테이블의 rowid 가 물리적인 row peices와 상응한다.
* Table Cluster 의 경우 다른 Table 에 있지만 같은 데이터 Block 존재하면 Row id 가 같다.
    * 이럴 수 있는게, 특정 컬럼이 같은 컬럼으로 묶여 있다면 가능할지도...

### Storage of Null Values

* null 은 DB 에 저장한다. 
    * 1 byte 저장. 저장하는 데이터는 길이가 0 이라는 1byte 임.
* 테이블의 마지막 3개의 컬럼이 null 이면, 이 컬럼들에 데이터를 저장하지 않는다.
    * 이걸 Trailing Nulls in Rows 라고 한다.

## Table Compression

* 공간절약을 위한 Table Compression.

### Basic Table Compression and Advanced Row Compression

* https://positivemh.tistory.com/340
* Dictionary based table Compression 은 좋은 압축비율을 제공한다.
* 2 가지 타입이 있다.
    1. Basic Table Compression : direct path INSERT, Alter table move, online table redefinition
    2. Advanced Row Compression : 일반적인 dml 로 수행.

* 하나의 Row 는 같이 저장한다. 
* DB 는 Short Reference로 중복값을 대체한다. 
* Table Space, Table, Partition에 압축을 선언 가능.

### Hybrid Columnnar Compression 

* 컬럼의 같은 값으로 Grouping 해서 저장한다.

#### Types of Hybrid Columnar Compression

* 2가지 타입의 압축이 있다.
1. Warehouse compression
2. Archive Compression

* exadata 에서 된다.

#### Compression Units 

* row 의 집합을 저장.

#### DML and Hybrid Columnar Compression 



