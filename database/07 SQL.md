# SQL

## Introduction to SQL

## Overview of SQL Statements

없음.

## Overivew of the Optimizer

### Use of the Optimizer

* execution plan : SQL 실행 계획
  
  * 계획을 위한 데이터는 여러가지 가 존재.
    
    * 쿼리 조건, 통계치, 힌트 등..

* 옵티마이저는 실행 계획을  아래와 같은 순서로 생성
  
  * 표현, 조건이 맞는지 확인.
  
  * Data Integrity 확인.
  
  * Statement Transformation
  
  * optimizer 목표 선택
  
  * optimizer access path 선택
  
  * join 순서 선택.

* 여러 실행 계획 중에서 가장 Cost 가 낮은 계획으로 진행. 

* optimizer 목표
  
  * `ALL_ROWS` : SQL 의 모든 Row 를 가져온다. (Batch 작업에 유리)
  
  * `FIRST_ROWS`  : 응답속도를 위해 첫번째 Row 를 가져온다. (online 작업에 유리.)

#### Optimizer Components

![](D:\github\jobtodo\Image\2022-03-05-08-41-39-image.png)

* 옵티마이져는 Parse 가 끝난 쿼리를 받고 여러 실행 가능한 Plan 을 생성한다.
* 각 Plan 은 통계치를 활용해 Cost 를 산정.
* 가장 낮은 Cost 의 Plan 을 query plan 으로 선택한다.

#### Query Transformer

* 좋은 실행계획을 세우기 위해, Oracle 은 쿼리를 여러 Query block으로 재구성 한다. 

#### Estimator

* 실행계획의 전체 Cost 를 계산하는 역할을 한다.

* 3가지 기준으로 계산한다.
  
  * Selectivity : 데이터의 선택도. 전체 인구 중에 설별 데이터는 선택도가 2. (좋지 않다.)
  
  * Cardinality : 분포도. 인구 100만 에서 성별 데이터 분포도는 50만(100 만 * Selectivity(2))
  
  * Cost :   disk I/O, CPU Usage and memory usage

#### Plan Generator

* 위 Query Transformator 를 통해서 여러 query block들을 조합해서 Plan 을 생성한다.

* 만약 현재 Plan 보다 더 좋은 Plan 이 있으면 SQL Plan Management(SPM) 이 더 좋은 Plan을 선택해서 실행한다.
  
  * 예측 실행계획이 반드시 실제 실행 계획은 아니다.

* `EXPLAIN PLAN`문장으로 옵티마이져가 선택한 실행계획을 볼수 있다.

### Access Path

* 정의 : DB 에서 데이터를 추출하는 방법.
* 방법
  * Full Table Scan : High water mark 까지 전체 전체 스캔.
  * Rowid Scans : 
  * Index Scans
  * Cluster scans
  * Hash scans

#### Optimizer Statistics

* 정의 : DB 와 OBject 의 상세 데이터들의 집합

* `dbms_stats` 패키지로 수집이 가능하다.

* 종류
  
  * Table statistics : rows, blocks, avg row length
  
  * Column statistics :  distinct value, null, bucket
  
  * Index statistics :  leaf blocks, index level
  
  * System statistics :  CPU and I/O

### Optimizer Hints

* Hint 로 Optimizer 의 plan 을 바꿀수 있다.

## Overview of SQL Processing

### Stages of SQL Processing

![](D:\github\jobtodo\Image\2022-03-06-07-41-13-image.png)

### SQL Parsing

* SQL 문장들을 데이터 구조로 분리하는 작업.
* SQL 문장을 DB 에 요청하면, Parse Call 을 요청한다. 
  * Parse Call 은 Cursor 를 생성한다.
    * Cursor 는 Session 별 private SQL area 를 담당한다.
      * private SQL area 는 Parse 가 끝난 SQL 문장을 저장한다.
* 3가지를 check 한다.
  * syntax check : 문법 체크
  * semantic check : 오브젝트 존재 여부.
  * shared pool check  : Hard Parse 를 줄이기 위한 작업.

#### Shared Pool Check

![](D:\github\jobtodo\Image\2022-03-06-08-03-03-image.png)

* DB 는 Hashing 알고리즘을 생성해 모든 SQL 에 SQL_ID 를 생성한다.
  
  * `v$sql.sql_id`

* DB 는 shared SQL area 에서 이미 Parse 가 끝난 문장을 찾아본다.

* V$SQL 의 hash value 들은 아래 이유로 달라질수 있다.
  
  * Memory address for the statement
  
  * Hash value of an eecutionplan for the statement

* Parse 의 종류
  
  * Hard Parse
    
    * DB 가 새로운 문장을 Parse 요청할 때 발생. 
      
      * `Library Cache Miss `라고도 함. DDL 은 언제나 Hard Parse 로 진행.
    
    * Library Cache 와 Data Dictionary 에 접속. 
      
      * Latch가 발생한다.
  
  * Soft Parse
    
    * 재사용이 가능한 문장이 DB 메모리에 존재.
      
      * `Library Cache Hit` 라고도 함.
    
    * session shared SQL area 는 Lath 발생을 줄일 수 있다.

* Hard Parse 가 발생할수 있는 상황

```sql
ALTER SYSTEM FLUSH SHARED_POOL;
SELECT * FROM my_table;

ALTER SESSION SET OPTIMIZER_MODE=FIRST_ROWS;
SELECT * FROM my_table;

ALTER SESSION SET SQL_TRACE=TRUE;
SELECT * FROM my_table;
```



### SQL Optimization

* 없음.

### SQL Row Source Generation

![](D:\github\jobtodo\Image\2022-03-06-08-16-55-image.png)

* 옵티마이저로 부터 최적 실행 계획을 받아 Query Plan 을 생성하는 Software 다.

* Query Plan 의 각 단계는 row set 을 생성한다.
  
  * row set 의 row 는 다음 혹은 마지막 단계에서 활용할수 있다.

* row source 는 row Set 을 생성하는 Table, View, result of join, grouping operation.

* row source 는 row source tree 를 생성한다.

### SQL Execution

* SQL Engine 은 row source generator 가 만든 각 row source 를 실행한다.

* DB 는 만약 메모리에 데이터가 없다면 Disk에서 데이터를 읽어드린다. 
  
  * 이때 DB 는 Lock 이나 Latch를 획득한다.
    
    * 데이터의 Integrity 를 위해서
    
    * SQL 실행중에 생긴 모든 변화를 기록하기 위해서 

* 마지막 단계는 Cursor 를 Close 하는 단계다.

## How Oracle Database Processes DML

* 대부분의 DML 은 Query component 를 가진다.

* 쿼리에서 커서를 실행하면 Result Set 이라는 Row 집합에 결과가 나타난다.
  
  * Result Set 은 한번에 한 Row 나 집합으로 가져올 수 있다.
  
  * Fetch 단계에서 DB 는 행을 선택하고, 요청에 따라 정렬한다.
  
  * 마지막 Row 를 가져올 때까지 계속 Fetch를 진행한다.

## How Oracle Database Processes DDL

* DB 는 DDL 을 실행할 때, 여러 recursive call 을 호출한다.
  
  * DDL 을 날리기전 모든 내용을 Commit 한다.
  * 권한을 check 한다.
  * Tablespace 의 Quota 를 확인한다.
  * 중복 이름을 확인한다.
  * Data Dictionary 에 데이터를 입력한다.
  * 성공 시 Commit, 실패 시 Rollback 한다.

