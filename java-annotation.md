# 정리할 내용

* java annotation 이란?
  * 1.5 버젼부터 소개.
  * java annotation은 meta 데이터다.
  * annotation parsing tool 이나 compiler 에 의해서 파싱할 수 있다.
    * 또한 컴파일 or runtime 때 사용 시기를 결정할 수 있다.
* java annotation
  * META Annotations in java 
    * Documented
      * javadoc 이나 문서화를 위해서 사용할 때, api 문서를 만들 때 어노테이션에 대한 설명도 포함하도록 지정 가능.
    * Target
      * 적용 범위를 지정.
      * TYPE, METHOD, CONSTRUCTOR, FIELD.
      * 만약 TARGET이 적용되지 않았다면, 모든 영역에서 사용이 가능하다.
    * Inherited
      * 어노테이션 타입이 자동으로 상속되도록 설정.
      * 슈퍼클래스에서 inherited 를 설정하면 상속받은 자식 클래스는 자동으로 어노테이션을 상속한다.
    * Retention
      * 어노테이션이 사용할 수 있는 주기를 설정할 수 있다.
      * SOURCE, CLASS, RUNTIME 이 있다.
    * Repeatable
      * 하나의 class나 fiels에 반복적으로 어노테이션을 사용할 수 있도록 설정.
  * Built-in annotations in java
    * Override
      * 슈퍼 클래스에서 정의한 클래스내용을 다시 재정의할 때 사용.
    * Depreciated
    * Suppresswarnings
    * FunctionalInterface
    * SafeVarargs
      * generic 타입의 문제가 없다는 것을 보장하는 장치다.
        * 뭐야...그냥 경고를 숨겨주는 거 아니야?
* java annotation 만들기.
  * 활용 예제
* Excel 로 연동해서 header를 정의해볼까?

# 참고

* https://www.journaldev.com/721/java-annotations#java-annotations-parsing
  * 기본
* https://nesoy.github.io/articles/2018-04/Java-Annotation
  * 기본적인 예제
* https://hamait.tistory.com/316?category=79137
  * 시리즈.
* https://woowabros.github.io/experience/2020/10/08/excel-download.html
  * 우아한 형제들의 엑셀 어노테이션 개발.
* https://blog.sunimos.me/12
  * documented 
* https://jistol.github.io/java/2018/08/31/annotation-repeatable/
  * repeatabled
* https://www.baeldung.com/java-safevarargs
  * Safevarargs

