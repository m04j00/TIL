# REST API

> Representational State Transfer
> 

### REST

- 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미한다.
- HTTP URI를 통해 자원을 명시한다.
- HTTP Method를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.
- 자원 기반의 구조 설계를 중심으로 리소스가 있고 HTTP METHOD를 통해 리소스를 처리하도록 설계된 아키텍쳐이다.

### 면접 예상 질문에 답변해보기

1. REST API가 무엇인가요?
    
    HTTP URI를 통해 자원을 명시하고 POST, GET, PUT, DELETE와 같은 HTTP METHOD를 통해 해당 자원에 대한 CRUD OPERATION을 적용하는 것을 의미합니다. 
    
2. REST API를 사용하는 이유가 무엇인가요?
    
    플랫폼에 맞추어 새로운 서버를 만드는 수고를 들이지 않기 위해 서버 디자인이 필요하게 되었습니다. 그래서 SERVER와 CLIENT의 역할 분리하기 위해 사용합니다. 
    

### CRUD Operation

- [POST] Create
    - 데이터 생성
- [GET] Read
    - 데이터 조회
- [PUT] Update
    - 데이터 수정
- [DELETE] Delete
    - 데이터 삭제

### 구성 요소

- HTTP URI로 자원을 명시한다.
    - `/users/order-id/1`
- HTTP Method로 자원에 대한 행위를 나타낸다.
- 표현(Representations)
    - 현재는 json 사용한다.

### REST의 특징

1. server-client 구조
    - 클라이언트는 유저와 관련된 처리를, 서버는 REST API를 제공함으로써 각각의 역활이 확실하게 구분되고 일괄적인 인터페이스로 분리되어 작동할 수 있게 한다.
2. stateless - 무상태
    - 서버는 요청에 대한 처리만 해주면 되기 때문에 상태 정보를 기억할 필요가 없다.
3. cacheable - 캐시 처리 가능
    - 캐시 사용을 통해 응답시간이 빨라진다.
    - 트랜잭션이 발생하지 않아 전체 응답시간, 성능, 서버의 자원 이용률을 향상 시킬 수 있다.
4. layered system - 계층화
    - 클라이언트와 서버가 분리되어 중간에 프록시 서버, 암호화 계층 등 중간 매체를 사용할 수 있어 자유도가 높다.
5. uniform interface - 인터페이스 일관성
    - HTTP 표준에만 따른다면 모든 플랫폼에서 사용 가능하다.
    - URI로 지정한 리소스에 대한 조작을 가능하게 하는 아키택쳐 스타일을 말한다.
    - 특정 언어나 기술에 종속되지 않는다.
6. self-descriptiveness 자체 표현 구조 
    - json을 이용한 message format을 이용하여 직관적으로 이해할 수 있고 REST API 메시지만으로 그 요청이 어떤 행위인지 알 수 있다.

### 규칙

1. 동사보다는 **명사**, 대문자보다는 **소문자** 사용한다. 
    - 대소문자를 구분하지 않아서 소문자를 사용한다.
    - 컨트롤 자원을 의미하는 URI는 예외적으로 동사를 허용한다.
    
    ```jsx
    good - localhost:8080/run
    bad  - localhost:8080/Running
    ```
    
2. 마지막에 슬래시를 포함하지 않는다. 
    
    ```jsx
    good - localhost:8080
    bad  - localhost:8080/
    ```
    
3. 하이픈을 사용한다. 
    
    ```jsx
    good - localhost:8080/min-j
    bad  - localhost:8080/min_j
    ```
    
4. 파일 확장명은 URI에 포함하지 않는다. 
    
    ```jsx
    good - localhost:8080/photo
    bad  - localhost:8080/photo.jpg
    ```
    
5. 행위를 포함하지 않는다.
    - 메소드에 따라 행위가 결정된다.
    
    ```jsx
    good - localhost:8080/post/1
    bad  - localhost:8080/delete-post/1
    ```
    

### REST ful

- REST의 원리를 따르는 시스템을 의미한다.
- REST API의 설계 규칙을 올바르게 지킨 시스템을 REST ful이라 말할 수 있다.

### 참고

- [도메인 형식](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Identifying_resources_on_the_Web)
- [URL과 URI의 차이점](https://www.charlezz.com/?p=44767)
- [URI 경로 디자인](http://hungry-developer.blogspot.com/2014/06/rest-api.html)
- [REST API](https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80)
- [REST API](https://velog.io/@somday/RESTful-API-%EC%9D%B4%EB%9E%80)
