* Lock and Latch
  * 차이점은?
  * Latch(https://otsteam.tistory.com/105) : 여러 user들이 자료구조, Object, 그리고 files 에 접급하는 것을 조율하는 단순한 매커니즘. 
    * 메모리에 대한 Lock 머커니즘
    * SGA 내부의 데이터 구조에서만 적용.
    * DeadLock이 존재하지 않는다. 
  * Lock : 데이터의 일괄성 유지를 위한 메터니즘.
    * Object나 데이터들의 
* tablespace / datafile / table / block / segment
  * 데이터가 Insert 할 때, Segment 는 어떻게 들어가는가?
* enq : HW - contention
  * Initial 과 Extents
  * HWM 가 옮겨질 때 발생.
  * 여러  Session 이 Extents 요청할 때 다음 Session을 위해 대기(Lock)
  * Lock 이 존재하는 2가지 경우의 수가 존재.
    * extents
    * space reclaim
      * 비어 있는 공간을 줄이기 위해 압축.
  * 2가지 구별하는 
    * enq : HW - contention 가 발생 중,  dba_segments.extents 가 올라가면 Extents
    * enq : HW - contention 가 발생 중, extents이 올라가지 않으면  space reclaim.
* truss -efpl
  * e
  * f
  * p
  * l
* oracle 에서 프로세스를 알아보는 방법
  * OS : truss, procstack
  * Oracle : oradebug(짧은 c stack 확인.)
* Spin / Hang
  * Spin : 무한 Loop 발생. cpu 100%.
  * Hang : 다음 단계로 넘어가지 않을 경우. cpu IDLE.

----

* 업그레이드 절차서

----

* SQL Trace 의 Bind 분석 방법.
* TKPROF SORT 옵션 정리.
* cpu 100% 이슈 확인.
  * use + system = cpu 점유율
  * nmon => T => 2
    * CPU TOP 확인.
  * ps -ef | grep PID
  * Weblogic
    * 환경
      * oacore_server1
      * 모니터링 >  Tread
      * self-tuning 문제 확인.
* MSI 모드
  * AP1 과 AP2 의 차이점은 AP1 에는 Admin 서버가 존재한다.
  * AP1이 죽으면 Admin서버가 죽으면서,  AP2를 재기동할 수 없다.
    * 그래서 Admin 서버가 죽으면, 통신하지 않고 AP2를 실행하는 모드가 MSI 모드다.