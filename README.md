# 스프링 MVC - 기본 기능

## 로깅

### 로깅 라이브러리
- 스프링부트 라이브러리에는 스프링부트 로깅 라이브러리(`spring-boot-starter-logging`)이 기본적으로 포함되어 있음
- 스프링부트의 기본 로깅 라이브러리
    - SLF4J
        - 로그 라이브러리들을 통합하여 인터페이스로 제공해 주는 라이브러리
    - Logback
- SLF4J는 인터페이스고 로그 라이브러리 구현체를 선택

### 로그 선언
- `private Logger log = LoggerFactory.getLogger(getClass());`
- `private static final Logger log = LoggerFactory.getLogger(Xxx.class)`
- `@Slf4j`: 롬복

### 로그 호출
- `log.info("info log = {}", mylog);`

### 매핑 정보
- `@RestController` VS `@Controller`
    - `@Controller`
        - `String` 반환 값을 뷰 이름으로 인식
        - 뷰를 찾고 뷰를 렌더링
    - `@RestController`
        - 반환값을 HTTP 메시지 바디에 바로 입력

### 테스트
- 로그가 출력 되는 포맷
    - `2022-01-07 14:42:46.964  INFO 6205 --- [           main] hello.springmvc.SpringmvcApplication     : Started SpringmvcApplication in 1.098 seconds (JVM running for 1.676)`
    - 날짜 시간 로그레벨 프로세스ID 스레드명 클래스명 로그메시지
- 로그레벨
    -  `TRACE` - `DEBUG` - `INFO` - `WARN` - `ERROR`
        - 개발 서버는 `DEBUG`
        - 운영 서버는 `INFO`

### 로그 레벨 설정
- `application.properties` 파일에서 설정
    - `logging.level.root=info`
        - 전체 로그 레벨 설정
    - `logging.level.hello.springmvc=debug`
        - hello.springmvc 패키지부터 하위 로그 레벨 설정
- 기본 로그 레벨은 `INFO`

### `{}`
- `log.debug("data = " + data)`
    - 실행되는 레벨이 아니더라도 더하기 연산이 실행
    - 필요없는 곳에 리소스를 사용
- `log.debug("data = {}", data)`
    - 파라미터로 넘겨줌
    - 실행되는 레벨이 아니라면 연산이 발생하지 않음

### 로그의 장점
- 부가 적인 정보와 출력 모양 조정
- 개발, 운영과 같은 상황에 따라 로그를 조정
- 파일 등으로 로그를 만들 수 있고, 분할도 가능
- `System.out` 보다 좋은 성능

### 참고
- [SLF4J](http://www.slf4j.org)
- [Logback](http://logback.qos.ch)
- [스프링부트의 로그](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.logging)

## 요청 매핑

## HTTP 요청 - 기본, 헤더 조회

## HTTP 요청 파라미터 - 쿼리 파라미터, HTML Form

## HTTP 요청 파라미터 `@RequestParam`

## HTTP 요청 파라미터 `@ModelAttribute`

## HTTP 요청 메시지

## HTTP 응답

## HTTP 메시지 컨버터

## 요청 매핑 핸들러 어댑터 구조
