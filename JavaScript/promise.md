## Promise
- resolve(value): 일이 성공적으로 끝난 경우 그 결과를 나타내는 value와 함께 호출된다.
- reject(error): 에러 발생 시 에러 객체를 나타내는 error와 함께 호출된다. 
- new Promise에 의해 생성된 객체
    - property.state
        1. 처음엔 "pending"(보류)
        2.  resolve가 호출되면 "fulfilled"
        3.  reject가 호출되면 "rejected"
  - property.result
      1. 처음엔 undefined
      2.  resolve(value)가 호출되면 value
      3.  reject(error)가 호출되면 error
  - https://ko.javascript.info/article/promise-basics/promise-resolve-reject.svg
- executor
    - new Promise에 의해 자동으로 실행된다. 
    - 결과를 즉시 얻든 늦게 얻든 상관없이 상황에 따라 인수로 넘겨준 콜백 중 하나를 반드시 호출해야 한다.
    - 보통 시간이 걸리는 일을 수행하고 일이 완료된 후에 resolve나 reject를 호출하는데 이때 프로미스 객체의 상태가 변경된다. 

```ts
const promise = new Promise(function(resolve, reject) {
  // executor 
});

const promise = new Promise(function(resolve, reject) {
  // 1초 뒤에 일이 성공적으로 끝났다는 신호가 전달되면서 result는 '완료'가 됩니다.
  setTimeout(() => resolve("완료"), 1000);
});

const promise = new Promise(function(resolve, reject) {
  // 1초 뒤에 에러와 함께 실행이 종료되었다는 신호를 보냅니다.
  setTimeout(() => reject(new Error("에러 발생!")), 1000);
});
```

### 프라미스는 성공 또는 실패만 해야 한다. 
- executor는 resolve나 reject 중 하나를 반드시 호출해야 한다. 이때 **변경된 상태는 더 이상 변하지 않는다**
- resolve나 reject는 인수를 하나만 받고(혹은 아무것도 받지 않음) 그 이외의 인수는 무시한다는 특성도 있다. 

## then
- 기본이 되는 메서드
- 첫 번째 인수는 프라미스가 **이행**되었을 때 실행되는 함수이고, 여기서 **실행 결과**를 받는다.
- 두 번째 인수는 프라미스가 **거부**되었을 때 실행되는 함수이고, 여기서 **에러**를 받는다. 
```ts
promise.then(
  function(onfulfilled) { /* 결과(result)를 다룹니다 */ },
  function(onrejected) { /* 에러(error)를 다룹니다 */ }
);
```
## catch
- 에러가 발생한 경우만 다루고 싶은 경우에 사용한다.
```ts
promise.catch(alert);
// .catch(f)는 promise.then(null, f)과 동일
```
## finally
- finally 핸들러엔 인수가 없다.
- 프라미스 결과는 finally를 통과해서 전달된다.
## Ex
```ts
const promise = loadScript("https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.11/lodash.js");

promise.then(
  script => alert(`${script.src} 불러오기 완료`),
  error => alert(`Error: ${error.message}`)
);

promise.then(script => alert('또다른 핸들러...'));
```

## 프로미스 체이닝
https://ko.javascript.info/promise-chaining

## Promise와 Error handling
### 프라미스: then vs. catch
- .then은 결과나 에러를 다음 .then이나 catch에 전달한다.
- 첫 번째 코드엔 catch가 있지만 두 번째 코드엔 이어지는 체인이 없기에 에러가 발생한 경우 이 에러를 처리하지 못한다는 차이가 생긴다. 
```ts
// f1에서 에러가 발생하면 .catch에서 에러가 처리된다. 
promise.then(f1).catch(f2);

// f1에서 에러가 발생해도 f2에선 처리하지 못한다. 
promise.then(f1, f2);
```
### 암시적 try…catch
- 프라미스 executor와 프라미스 핸들러 코드 주위엔 암시적 try..catch가 있어 **예외 발생 시에 예외를 잡고 이를 reject처럼 다룬다**. 
- executor 주위의 '암시적 try..catch'는 스스로 에러를 잡고, 에러를 거부상태의 프라미스로 변경시킨다. 
```ts
new Promise((resolve, reject) => {
  throw new Error("에러 발생!");
}).catch(alert); // Error: 에러 발생!

=== 결과 동일 

new Promise((resolve, reject) => {
  reject(new Error("에러 발생!"));
}).catch(alert); // Error: 에러 발생!
```
### 체인 마지막 .catch
- .then 핸들러를 원하는 만큼 사용 후에 마지막에 .catch를 사용하면 .then 핸들러에서 발생한 모든 에러를 처리할 수 있다.
### .catch에서 에러 처리 성공 후에 .then
- .catch 안에서 throw를 사용하면 제어 흐름이 가장 가까운 곳에 있는 에러 핸들러로 넘어간다. 
여기서 에러가 성공적으로 처리되면 가장 가까운 곳에 있는 .then 핸들러로 제어 흐름이 넘어가 실행이 이어진다.
```ts
// 실행 순서: catch -> then
new Promise((resolve, reject) => {
  throw new Error("에러 발생");
}).catch(function(error) {
  alert("에러 처리 완료");
}).then(() => alert("다음 핸들러 실행"));

// 에러 처리 완료
// 다음 핸들러 실행
```
### setTimeout에서의 에러
- 아래 코드에서 에러는 executor가 실행되는 동안이 아닌 나중에 발생하므로 프로미스는 에러를 처리할 수 없다. 
```ts
new Promise(function(resolve, reject) {
  setTimeout(() => {
    throw new Error("에러 발생!");
  }, 1000);
}).catch(alert);
```
## 마이크로태스크
https://ko.javascript.info/microtask-queue
### 마이크로태스크 큐
- 비동기 작업을 처리하기 위한 내부 큐이다. 
- V8 엔진에서 마이크로태스크 큐라 부른다. 
- 마이크로태스크 큐는 먼저 들어온 작업을 먼저 실행한다(FIFO, first-in-first-out).
- 실행할 것이 아무것도 남아있지 않을 때만 마이크로태스크 큐에 있는 작업이 실행되기 시작한다. 
- 프로미스가 준비되었을 때 프로미스의 then/catch/finally 핸들러가 큐에 들어간다(고 보면 된다.).
현재 코드에서 자유로운 상태가 되었을 때 JS 엔진은 큐에서 작업을 꺼내 실행한다. 
```ts
const promise = Promise.resolve();
promise.then(() => alert("프라미스 성공"));
alert("코드 종료"); 

// 코드 종료
// 프라미스 성공
```
![image](https://github.com/m04j00/TIL/assets/69101860/bd6c1d18-5ab8-40fb-92cf-7e6eeb1746a4)

```ts
Promise.resolve()
  .then(() => alert("프라미스 성공!"))
  .then(() => alert("코드 종료"));
  
// 프라미스 성공
// 코드 종료
  ```
