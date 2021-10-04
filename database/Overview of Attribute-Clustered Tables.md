# Overview of Attribute-Clustered Tables

* 데이터를 저장하는 Heap Table
    * Disk 에서 User 가 정의한 지시를 기반으로
        * 근접성
* 지시는 하나 혹은 여러 테이블들의 컬럼들로 정의할 수 있다.
* 지시는 아래와 같이 정의한다. 
    1. `CLUSTERING...BY LINEAR ORDER` 는 특정 컬럼들에 따라 데이터를 정렬한다.
        * `BY LINEAR ORDER` 절을 고려해라
            * clustering 절에 명시한 컬럼들의 접두사? 
                * :blue_book:the prefix of columns 를 왜 고려해야 하는거지?
                * cust_id, prod_id 순서로 데이터를 모을 수 있다.
    2. `CLUSTERING ... BY INTERLEAVED ORDER` 는 데이터를 정렬한다.
        * :blue_book:뭔가 있는데, 잘 모르겠다.

## Advantages of Attribute-Clustered Tables

* I/O 감소가 제일 큰 이득이다.
* 데이터 압축

## Join Attribute Clustered Tables

* join attribute clustered tables 는 데이터를 저장한다.
    * table cluster 와 다르게 같은 DB Block에 저장하지 않는다. 
    *  

## I/O Reduction Using Zones

## Attribute-Clustered Tables with Linear Ordering

## Attribute-Clustered Tables with Interleaved Ordering

