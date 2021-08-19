* 요청까지는 만들 수 있었는데, 받는 걸 만들수가 없네...

* 어떻게 하면 받는것도 만들 수 있을까?

* 방법#1

  * Dom Parser 로 데이터를 가져온다
    * xpath 로 데이터를 parsing 하는 방법 확인. (O)
    * cdata 제거 방법 확인.
      * https://stackoverflow.com/questions/8045716/how-to-use-xpath-to-find-the-node-value-with-cdata-tag-in-java
      * 제거하는 방법은 못 찾았는데...여기서 바로 Parsing이 들어간다면?? (X)
      * 속상하네...방법이 없나 보네...
  * 코드 정리. (O)
  * xml 을 객체로 마샬링 하는 방법 찾기.
    * https://ryan-han.com/post/java/xml_to_java_object/
      * JAXBContext 와 Java Model
    * Rows 없이 한번 해보자.
      * 파싱 불가.
    * Rows 있이 Parsig은 성공

  * 가져온 결과를 다시 데이터 Object 로 만든다. (X)
    * 이게 더 어려워

----


* 데이터 구조는?
  * dsQueryResponse
    * Rows
      * Row
        * ows_fRecurrence
        * ows_WorkspaceLink 
        * ows_LinkTitle
        * ows_Location 
        * ows_EventDate
        * ows_EndDate
        * ows_fAllDayEvent
        * ows_ID
* 이것 말고 내가 해야할 일이 따로 있지 않을까?
  * 데이터가 맞는지 확인을 해봐야 한다. 
    * 어떤 식으로 확인을 해봐야 할까? 그러게...
  * 데이터 : 31 개
  * 칼랜더 : 32 개
* 기한 안에 이걸 다 확인할 수 있을까?
  * 데이터 Object 로 만들 수 있는 것만 확인하고 배포 진행을 요청하면 된다.

----

* 나으 고민...

  * 문자로 들어온 데이터를 어떻게 날짜로 바꿀 것인가?

    ``` java
    // 현재월의 첫번째 일자 - use Calendar Calendar calendar = Calendar.getInstance(); calendar.set(Calendar.DAY_OF_MONTH, 1); SimpleDateFormat simpleDateFormat = new SimpleDateFormat(DateUtil.YYYY_MM_DD); String startDate = simpleDateFormat.format(calendar.getTime()); System.out.println(startDate); // 2021-06-01 // 현재월의 첫번째 일자 - use LocalDate LocalDate firstDate = LocalDate.now().with(TemporalAdjusters.firstDayOfMonth()); String firstDateString = firstDate.format(DateTimeFormatter.ISO_DATE); System.out.println(firstDateString); // 2021-06-01 // 현재월의 마지막 일자 LocalDate lastdDate = LocalDate.now().with(TemporalAdjusters.lastDayOfMonth()); String lastDateString = lastdDate.format(DateTimeFormatter.ISO_DATE); System.out.println(lastDateString); // 2021-06-30 // 현재월의 첫번째 월요일인 날짜. LocalDate dateOfFirstMonday = LocalDate.now().with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY)); System.out.println(dateOfFirstMonday); // 2021-06-07
    ```
    
    * 대소 비교(O)
    
      * https://hianna.tistory.com/611
      * 1:12  isAfter 1:13 == false
        1:12 > 1:13 == false
      * End Date > 8.1 == true
      * 6.30 < Start Date == true
    
  * 데이터 검색을 어떻게 할 것인가? (O)

  * Excel 로 어떻게 만들 것인가?

    * https://ddasi-live.tistory.com/47
      * 참고
    
  * 날짜가 잘못 증가하고 있는데?
  
    * if 문장을 잘못 적용함
  
  * 데이터 검색(O)
  
  * 각 날짜별로 맞는 일정은 어떻게 등록할 것인가?
  
    * 먼저 기간컬럼을 확인.
      * https://www.daleseo.com/java8-duration-period/
        * 날짜들의 사이기간을 등록.
    * 8/12 8/13 = 1
      * 8/1~8/3
        * 8/2, 8/1  = -1
        * 8/2, 8/2 = 0
        * 8/2, 0/3 = 1
        * `> 2` 면 종료.
      * 하루 뒤
        * 8/3, 8/1  = -2
        * 8/3, 8/2 = 1
        * 8/3, 0/3 = 0
        * `> 2` 면 종료.
    * 왜 윤기형 차장님이 2번 나오는 거지?
  
  * 다른 접근법을 사용해보자.
  
    * 해당 달의 주수를 구하자
      * https://jang8584.tistory.com/188
        * 주수를 구하자
    * 그 주에 해당하는 시작과 종료일자를 구한다.
      * 반복한다.
    * 글자 색깔
      * https://stackoverflow.com/questions/15730146/how-to-change-font-color-of-particular-cell-apache-poi-3-9/15738424
      
    
  * 엑셀을 뽑아보자.
  
    * 왜 null 이지?
  
  * 데이터 추출 완료.

---

- [x] pom.xml
- [x] bean
- [x] request
- [x] response
- [x] service
- [x] excel
- [x] controller

---

* Wels 에게 다운받은 String 이 EUC-KR  로 다운로드 받는다.
  * 어떻게 처리해야 할까?
  * byte array 로 데이터를 다운 받아서 다시 인코딩한다?
    * 운영에서 테스트
  * 만약 인코딩을 바꾸지 않은 상태에서 bytes 만 던지면 어떻게 될 것 인가?
    * 성공
* 만약 캘린더에 등록되지 않은 날짜를 던진다면?
  * 캘린더와 리스트는 존재한다.
* 로그 정리 시작.

