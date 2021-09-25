# Overview of Table Clusters

* Table cluster is a group of tables that share common columns
* 만약 테이블들이 cluster 하면, 하나의 data block 은 여러 테이블들의 row 를 가진다.
    * 물리적으로 1개의 block에 저장.
* cluster keys 는 cluster table 이 가진 공통 컬럼들이다.
* 장점은 아래와 같다.
    1. Distk IO가 줄어든다.
    2. 클러스터 테이블들의 조인 시간이 향상된다.
    3. 테이블과 인덱스를 저장 공간을 절약할 수 있다. 왜냐하면 cluster key value 는 각 Row 에 반복적으로 저장할 필요가 없다. 

* 이런 상황에서는 좋지 않다.
    1. 테이블이 자주 업데이트 된다.
    2. 테이블이 자주 Full Table Scan이 필요하다.
    3. 테이블에 Truncate 가 필요하다.

## Overview of Indexed Clusters

* index cluster 는 index 를 사용하는  table cluster 다.
* 데이터가 들어가기 전에 미리 만들어야 한다.
* `HASHKEY`절을 선언하지 않았다면, index clsuter 가 된다. 

* B-트리 클러스터 인덱스는 클러스터 키 값을 데이터가 포함된 블록의 DBA(데이터베이스 블록 주소)와 연결합니다.
* cluster index 는 cluster table 과 별도 관리된다.

## Overview of Hash Clusters

* hash clsuter 는 hash function 을 사용한 index cluster 다.
    * data 가 index 다.
* hash function의 결과는 cluster 에 있는  data block 이다.
* 이럴 때 사용하면 좋다.
    1. 수정보다 조회가 많을 경우
    2. '=' 조건으로 자주 조회하는 컬럼을 hash key column으로 사용할 경우.
    3. hash key 의 기준값이 예측이 가능할 경우.

### Hash Cluster Creation

* `HASHKEY` 구절에서 몇개의 Hash Key Value 를 수용할 지 ㅈ결정한다.
* bucket
    * https://velog.io/@agugu95/Hash-Table-Java-HashMap
    * 여기에 Key 와 Database Block Address 가 들어있다.

### Hash Cluster Queries

* hash clster 는 equal 조건만 가능하다.

### Hash Cluster Variations

* Single Table hash cluster 
* Sorted hash cluster 

### Hash Cluster Storage

* DB 는 공간을 아래와 같이 계산한다.

> HASHKEYS * SIZE / database_block_size
> 

* Hash Key 가 100 이지만 200 개를 넣을 수는 있다.
    * 단지 앞으로 저장 공간에 효율이 떨어진다.
    * hash collision 이 발생한다.
        * 다른 Key 값이 같은 Hash value 를 가지는 경우.
    * 만약 기존 key 값의 데이터가 공간을 다 사용하고 있다면 새로운 Key 를 위한 공간을 할당받는다.
        * 문제는 앞으로 기존 or 새로운 Key 조회는 언제나 2개 이상의 Block을 조회한다.