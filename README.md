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
### `@Controller` VS `@RestController`
 - `@Controller`는 반환 값이 `String`이면 뷰 이름으로 인식하여 뷰를 찾고 렌더링
 - `@RestController`는 뷰를 찾지 않고 반환 값을 HTTP 메시지 바디에 바로 입력

 ### `@RequestMapping`
 - URL 호출이 오면 매핑되는 메섣그 ㅏ실행
 - 배열로 여러개의 URL을 받을 수도 있음

### 같은 요청으로 매핑
- `/hello`와 `/hello/`는 다른 URL이지만 스프링은 같은 요청(`/hello`)으로 매핑

### HTTP 메서드
- `@RequestMapping`에 `method` 속성으로 HTTP 메서드가 지정되어 있지 않다면 HTTP 메서드와 무관하게 호출
- `GET`만 허용한다면 `@RequestMapping(value = "/hello", method = RequestMethod.GET)`와 같은 식으로 method 지정
    - `@GetMapping(value="/hello")`도 같은 의미
- 허용 되지 않는 메서드일 때는 HTTP 405 `Method Not Allowed` 반환

### `PathVariable`
- 경로에 식별자를 사용
- `@GetMapping("/mapping/{userId}")`는 `userId`를 식별자로 사용
- `@PathVariable("userId")`로 값을 가지고 올 수 있음
- `@PathVariable`의 이름과 파라미터 이름이 같으면 생략 가능

### 파라미터 조건 매핑
- params = "country=korea"
    - 파라미터에 `country=korea`가 있을 때 매핑
    - `@GetMapping(value = "/mapping-param", params = "country=korea")`
    - `localhost:8080/mapping-param?country=korea`인 경우 매핑

### 헤더 조건 매핑
- `@GetMapping(value = "/mapping-header", headers = "mode=debug")`

### 미디어 타입 조건 매핑
- `@PostMapping(value = "/mapping-consume", consumes = "application/json")`
- `@PostMapping(value = "/mapping-produce", produces = "text/html")`

## HTTP 요청 - 기본, 헤더 조회
- `HttpMethod`: HTTP 메서드를 조회
- `Locale`: locale 정보 조회
- `@RequestHeader MultiValueMap<String, String> header`: 헤더를 `MultiValueMap` 형식으로 조회
    - `MultiValueMap`
        - 하나의 키에 여러 값을 가지는 Map
        - `header.get("key")` 방식으로 값 조회
- `@RequestHeader("host") String host`: 특정 HTTP 헤더 조회
- `@CookieValue`: 쿠기 조회
- 속성
    - `required`
        - 필수 값인지 아닌지 지정
        - `true`/`false` 값을 가짐
    - `defaultValue`
        - 기본값 지정

### `@Slf4j`
- `Logger`을 자동으로 생성
- `log`로 사용 가능

[`@Controller`의 파라미터 목록](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-arguments)
[`@Controller`의 응답 값 목록](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-return-types)

## HTTP 요청 파라미터 - 쿼리 파라미터, HTML Form
- `GET`
    - 쿼리 파라미터
- `POST`
    - `content-type: application/x-www-form-urlencoded`
    - 메시지 바디에 쿼리 파라미터 형식으로 전달
- `HTTP message body`
    - message body에 데이터를 직접 담아 요청
    - HTTP API에서 사용
    - POST, PUT, PATCH

### 쿼리 파라미터 조회
- `HttpServletRequest`의 `getParameter()`

## `@RequestParam`
- `@RequestParam("파라미터의 이름") String name`으로 조회
    - 파라미터 이름과 변수 이름이 같다면 `"파라미터의 이름"` 생략 가능
    - 단순 타입(String, int, Integer ...)이면 `@RequestParam`도 생략 가능
        - `@RequestParam`이 생략 되면 `required=false`가 적용 됨
- `?name=`: 파라미터 이름은 있지만 값이 없는 경우
    - 빈 문자열로 인식
    - `defaultValue` 사용: 빈 문자의 경우 기본값이 적용
- primitive type에는 `null` 입력이 불가능
    - `int`이면 `Integer`로 변경
    - `defaultValue` 사용

### Map으로 파라미터 조회
- `Map` 혹은 `MultiValueMap`을 사용하여 조회
    - `@RequestParam Map`: 파라미터에 대응하는 값이 1개
    - `@RequestParam MultiValueMap`: 파라미터의 값이 중복

> `@ResponseBody` View 조회를 무시. HTTP message body에 직접 내용 입력.


## `@ModelAttribute`
- 파라미터의 값을 가지는 객체 생성
- `@ModelAttribute MyData data`와 같이 사용
- 실행 과정
    - `MyData` 객체 생성
    - 파라미터의 이름에 대응되는 값에 `setter`을 사용하여 값을 바인딩

### `@Data`
- 롬복
- `@Getter` + `@Setter` + `@ToString` + `@EqualsAndHashCode` + `@RequiredArgsConstructor`

### `@ModelAttribute` 생략
- `@ModelAttribute`도 생략 가능
- 생략시 규칙
    1. `String`, `int`, `Integer`와 같은 단순 타입 -> `@RequestParam`
    2. 나머지 -> `@ModelAttribute`

## HTTP 텍스트 요청 메시지
- HTTP 메시지 바디를 통해 데이터가 넘어오는 경우 `@RequestParam`, `@ModelAttribute` 사용 불가능
    - HTML Form 형식인 경우는 예외

### `HttpServletRequest request`
- `InputStream`을 사용하여 데이터를 직접 읽을 수 있음
    1. `ServletInputStream inputStream = request.getInputStream()`
    2. `StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8)`

### `InputStream inputStream`
- `InputStream`을 바로 받을 수 있음
    - `StreamUtils.copyToString` 과정 필요

### 스프링 MVC가 지원하는 파라미터
- `InputStream(Reader)`
    - HTTP 요청 메시지 바디의 내용을 직접 조회
- `OutputStream(Writer)`
    - HTTP 응답 메시지의 바디에 직접 결과 출력
- `HttpEntity`
    - 조회
        - header, body 정보 조회
        - 메시지 바디 정보를 직접 조회
        - 파라미터 조회 기능 X
    - 응답
        - 메시지 바디 직접 반환
        - 헤더 정보도 포함 가능
        - view 조회 X
    - `HttpEntity`를 상속받은 객체
        - `RequestEntity`
        - `ResponseEntity`

### `@RequestBody`
- HTTP 메시지 바디 정보 조회
    - 헤더 정보: `HttpEntity`, `@RequestHeader`
- 요청 파라미터와는 관계 없음

### 요청 파라미터 & HTTP 메시지 바디 조회
- 요청 파라미터 조회
    - `@RequestParam`
    - `@ModelAttribute`
- HTTP 메시지 바디 조회
    - `@RequestBody`

### `@ResponseBody`
- 응답 결과를 HTTP 바디에 직접 담아서 전달
- view를 사용하지 않음

## HTTP JSON 요청 메시지

### `ObjectMapper` 사용
- 메시지 바디를 직접 읽어 변환
-  `objectMapper.readValue(messageBody, MyData.class)` 와 같이 사용

### `@RequestBody String`
- HTTP 메시지에서 데이터를 꺼냄

### `@RequestBody` + 객체
- `@RequestBody MyData`
- 직접 만든 객체를 지정할 수 있음
- 메시지 컨버터가 변환 해줌

> `@RequestBody`가 생략되면, `@ModelAttribute`가 적용되어 요청 파라미터를 처리하게 됨

### `@ResponseBody`
- 객체를 HTTP 바디에 넣을 수 있음

### `@RequestBody`, `@ResponseBody`와 객체
- `@RequestBody`
    - JSON -> HTTP 메시지 컨버터 -> 객체
- `@ResponseBody`
    - 객체 -> HTTP 메시지 컨버터 -> JSON

## HTTP 응답 - 정적 리소스, 뷰 템플릿

### 스프링의 응답 데이터
- 정적 리소스
    - HTML, CSS, Javascript 같은 리소스
- 뷰 템플릿
    - 동적인 HTML 제공
- HTTP 메시지
    - HTTP API를 제공하는 경우에 데이터를 전달

### 정적 리소스
- 스프링 부트는 클래스패스에 다음 디렉토리에 있는 정적 리소스 제공
    - `/static`
    - `/public`
    - `/resources`
    - `/META-INF/resources`
- `src/main/resources`
    - 리소스를 보관하는 곳
    - 클래스패스의 시작 경로
- 정적 리소스 경로: `src/main/resources/static`
    - `src/main/resources/static/hello/spring.html`: `localhost:8080/hello/spring.html`에 접속
- 파일을 변경없이 제공

### 뷰 템플릿
- 뷰 템플릿 경로: `src/main/resources/templates`
- 뷰 템플릿을 리턴하는 방법
    - `ModelAndView`
        - 뷰 템플릿의 경로를 `ModelAndView`에 넣고 `addObject`로 데이터를 추가
    - `String`
        - `@ResponseBody`가 없다면 뷰 리졸버가 실행되어 뷰를 찾고 렌더링
        - `@ResponseBody`가 있다면 메시지 바디에 직접 문자를 입력
    - `Void`
        - `@Controller`를 사용하고 HTTP 바디 처리하는 파라미터가 없을 때, URL을 참고하여 뷰를 제공

### HTTP 메시지
- `@ResponseBody`, `HttpEntity`를 사용하여, 메시지 바디에 직접 응답 데이터 출력

### Thmeleaf 스프링 부트 설정 
1.  `build.gradle`에 필요한 스프링 빈 등록
``` gradle
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
```
2. `application.properties`에 필요한 값 설정
```
# 기본 값
pring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
```

## HTTP 응답 - HTTP API
- HTTP API의 경우 HTML이 아닌 데이터 전달
- 전달하는 방법
    - `HttpServletResponse` 객체 사용
    - `ResponseEntity` 엔티티 사용
    - `@ResponseBody` 사용
    - `@RestController`: 모두 `@ResponseBody`가 적용되는 효과


## HTTP 메시지 컨버터

## 요청 매핑 핸들러 어댑터 구조
