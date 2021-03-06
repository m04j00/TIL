> Node.js는 Javascript의 런타임이다. Non-blocking I/O와 단일 스레드 이벤트 루프를 통한 높은 처리 성능을 가지고 있는 특징이 있다.

## 서버

- 서버는 네트워크를 통해 클라이언트에 정보나 서비스를 제공하는 컴퓨터 또는 프로그램이다.
- 웹이나 앱을 사용할 때 데이터가 생성된다. 이 데이터를 어딘가에 저장하고, 어딘가에서 클라이언트로 데이터를 받아오는 것이 서버이다.
- 요청에 대한 응답도 하고 다른 서버에 요청을 보낼 수도 있다.

- 서버는 클라이언트의 요청에 대해 응답을 한다.
- 노드는 자바스크립트 프로그램이 서버로서 기능하기 위한 도구를 제공하므로 서버 역할을 수행할 수 있다.

## 자바 스크립트 런타임

- 런타임은 특정 언어로 만든 프로그램들을 실행할 수 있는 환경을 뜻한다.
- 노드는 자바스크립트 프로그램을 컴퓨터에서 실행할 수 있다
- 노드는 자바스크립트의 실행기라 봐도 무방하다.

## 이벤트 기반

- 이벤트가 발생할 때 미리 지정해둔 작업을 수행하는 방식 의미
- 클릭이나 네트워크 요청이 있다.
- 특정 이벤트가 발생할 때 무엇을 할지 미리 등록해야 하는데, 이벤트 리스너에 콜백 함수를 등록한다고 표현한다.

- ex) 버튼을 클릭할 때 경고창을 띄우도록 설정
  - 클릭 이벤트 리스너에 경고창을 띄우는 콜백 함수를 등록하면 클릭 이벤트가 발생할 때마다 콜백 함수가 실행되어 경고창이 뜬다.

### 이벤트 루프

- 여러 이벤트가 동시에 발생했을 때 어떤 순서로 콜백 함수를 호출할지를 이벤트 루프가 판단한다.
- 노드가 종료될 때까지 이벤트 처리를 위한 작업을 반복하므로 루프라 부른다.

## 논 블로킹 I/O

- I/O 작업은 동시에 처리할 수 있다.
- 노드는 논 블로킹 방식으로 처리하는 방법 제공한다.
- 논 블로킹은 이전 작업이 완료될 때까지 대기하지 않고 다음 작업을 수행함을 의미한다.
- 블로킹은 이전 작업이 끝나야만 다음 작업을 수행하는 것을 의미한다.
- 블로킹 방식보다 논 블로킹 방식이 같은 작업을 더 짧은 시간에 처리할 수 있다.
- 노드는 I/O 작업을 백그라운드로 넘겨 동시에 처리한다. 그래서 동시에 처리될 수 있는 작업들을 최대한 묶어 백그라운드로 넘겨 시간을 절약할 수 있다.
- I/O 작업이 없다고 해서 논 블로킹이 의미가 없는 게 아니라 오래 걸리는 작업을 처리해야 하는 경우 논 블로킹을 통해 실행 순서를 바꿔줌으로써 간단한 작업들이 대기하는 상황을 막을 수 있다는 점에서 의의가 있다.
- !! 논블로킹과 동시는 같은 의미가 아니다. 동시성은 동시 처리가 가능한 작업을 논 블로킹 처리해야 얻을 수 있다.
- 동기와 비동기, 블로킹과 논 블로킹은 유사하다.

## 싱글 스레드

- 싱글 스레드란 스레드가 하나뿐이라는 것을 의미한다.
- 자바스크립트 코드가 동시에 실행될 수 없는 이유이기도 하다.
- 프로세스
    - 운영체제에서 할당하는 작업의 단위
    - 노드나 웹 브라우저 같은 프로그램은 개별적인 프로세스이다.
    - 프로세스간에는 메모리 등의 자원을 공유하지 않는다.
- 스레드
    - 프로세스 내에서 실행되는 흐름의 단위
    - 프로세스는 스레드를 여러 개 생성해 여러 작업을 동시에 처리할 수 있다.
    - 스레드들은 부모 프로세스의 자원을 공유한다.
    - 같은 주소의 메모리에 접근 가능하므로 데이터를 공유할 수 있다.
- 엄연히 말하면 싱글 스레드로 동작하지 않는다.
    - 노드를 실행하면 먼저 프로세스가 하나 생성된다. 그 프로세스에서 스레드들을 생성하는데 이때 내부적으로 스레드를 여러개 생성한다. 그 중에서 직접 제어할 수 있는 스레드가 하나 뿐이라 싱글 스레드라 여겨진다.
- 요청이 많이 들어오면 한 번에 하나씩 요청을 처리한다. 블로킹이 심하게 일어나는 작업을 처리하지만 않는다면 스레드 하나로 충분하다.
