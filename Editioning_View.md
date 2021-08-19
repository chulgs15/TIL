누가 언제 어디서 무엇을 어떻게 왜?

* 현재 상황
  * 오래간만에 글을 올린다. 현재 Project를 동시에 2개에 물려 있어. 손절이 안되고 있다.
  * 간만에 본업에 관련된 정보를 정리한다. 현재 Site에서 Oracle EBS Upgrade 중이다.
  * 새로운 개념이 도입되어 약간의 혼선(?)이 있어 정리하는 글을 올려본다.
* Edition Based Redefinition (EBR)

  * 새로운 Oracle EBS 는 EBR을 도입한다. 기존 방식처럼 Table을 CRUD 하면 오류가 발생할 수 있다고 한다. 

    * 이 오류가 왜 생기는지 이해하기 힘들어, 다시 정리하고자 한다. 
  * Oracle 은 이런 개념을 왜 적용한 이유는 Application 업그레이드의 Downtime을 줄이기 위해서다.
    * 그렇다면 이 개념이 어떻게 Downtime 을 줄일 수 있는가?
  * 먼저 Edition에 대해서 알아보자.
    * Edition은 DB Object 에게 붙이는 라벨이다. 
      * 가수의 앨범은 한정판과 일반판이 있다면, 한정판은 일반판에 커버, 브로마이드..를 더 추가해서 발매한다. 같은 노래가 들어 있지만, 구매자가 특별한 의미를 가지도록 구성을 바꾼다.
        * 이런 앨범 처럼 Table, View, Procedure...들을 용도에 따라 라벨별로 구성할 수 있다.
      * DB Object에 새로운 Edition 이 만들면, 하나의 Child Edition이 존재한다.
        * 참고로 Alter Session 명령어로 Edition을 바꿀 수 있다.
      * 오해 부분이 있을 수 있는데, Version 관리 처럼 객체의 마지막 Edition이 가장 활용하기 좋은 Edition 이 아닐 수도 있다. 
  * 그렇다면 이 Edition 을 이용하면 어떻게 Downtime을 줄일 수 있을 것인가? 아직 Patch를 적용하지 않아서 생각히면...
    * 경험 상, EBS 에  Patch를 적용하려면 DB를 재기동해야 하는 케이스가 있다.
    * Database를  Patch 전/후 Edition을 만들어, DB를 종료하지 않고 패치를 적용한다면? 그리고 Patch 후 Edition으로 운영하면?  (RAC와 다른 이중화의 느낌이다.)
* 하지만 모든 변화는 양날의 칼이 존재했다. 
  * 우리의 기존 Application 들은 아래와 같은 그림으로 운영해 왔다.
  * 하지만 Edition 적용으로 아래와 같이 변화가 발생했다.
  * 만약 여기서 EBS 나 Legacy 시스템들이 붙게 되면??
    * 데이터 일관성에 문제가 발생한다.
  * 우리의 이슈는 무엇인가? 
  * 스키마 제거 이슈를 정리.
* Edition 객체 정리
* Trigger 는 어떻게 이동시켜 주는가?
  * 겁나 무식하지만...모든 문자열을 연결해야 가능.
* 과거 Edition으로 돌아가는 것이 가능한가?
  * 가능한 예제를 보여주자.
* 만약 테이블 이름이 30글자면 어떻게 처리하는가?
  * ERP 한정으로 확인해보자.
* 만약 테이블 컬럼 이름이 30글자 이상이면 어떻게 처리하는가?
  * ERP 한정으로 확인해보자.



# 참고사항

*  https://k21academy.com/oracle-e-business-suiter12-2-upgrade/edition-based-redefinition-in-oracle-ebs-r12-2/
   *  개념 정리.
*  https://www.morganslibrary.org/reference/editioning_views.html
   * 개념 정리
*  https://oracle-base.com/articles/11g/edition-based-redefinition-11gr2
   * 개념 정리
*  https://k21academy.com/oracle-e-business-suiter12-2-upgrade/oracle-ebs-12-2-9-vision-instance-on-oracle-cloud-oci/
   * Oracle Cloud 에서 EBS 해보는 방법.

---

* 11gR2 에서 탄생한 새로운 기능이다.
  * Downtime 을 줄이면서 Application 의 업그레이드를 위해.
  * EBS에서는 Online Patch 작업이 가능하다.
    * 시스템을 실행 중에 패치가 가능하다.
* EBR
  * 고 가용성이 중요하다.
    * RAC, Dataguard 쓰면 되는거 아니야?
      * 돈 주고 사야 해....
      * 그리고 Application 이나, 조직이 바뀌는 건 저 2개로 할 수 없어.
        * 왜냐하면 데이터와 컬럼이 바뀌는 작업이잖아?
* EBR 의 장점
  * PL/SQL 객체들의 여러 버젼을 하나의 Schema에서 확인 할 수 있어.
  * 사용자는 알 수 없게 이전, 이후 버젼을 자유롭게 변경할 수 있어.
* Editions
  * DB 의 기능. Owner 가 없는 Object 임.
  * 모든 DB 는 적어도 1개의 base Edition 이 존재함.
  * 어떤 새로운 Edition 은 적어도 하나의 Child Edition 이 존재해.
    * Session Command 로 edition을 바꿀 수 있어.
  * edition 가능한 객체들에게 붙이는 Version 라벨.
* editioning view
  * 하나의 테이블의 컬럼들의 집합. 
  * alias 로 접근.
* Cross Eddition Triggers
  * 두가지 타입으로 나뉜다. Forward, Backward.
  * Forward : Old Edition 에서 사용한 컬럼의 데이터 -> New Edition 에서 사용한 컬럼 데이터
  * Backward : New Edition 에서 사용한 컬럼 데이터 => Old Edition 에서 사용한 컬럼의 데이터



























