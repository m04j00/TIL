## async 함수
- async는 함수 앞에 위치하고, 해당 함수는 항상 프로미스를 반환한다. 함수가 프로미스가 아닌 값을 반환해도 이행 상태의 프로미스(resolved promise)로 감싸 이행된 프로미스가 반환되도록 한다. 

```ts
  // f(): Promise<number>
  async function f() {
    return 1;
  }

  f().then(alert); // 1
  ```
  
## await
- 자바스크립트는 await 키워드를 만나면 프로미스가 처리될 때까지 기다린다. 
- 프라미스가 처리되길 기다리는 동안엔 엔진이 다른 일(다른 스크립트를 실행, 이벤트 처리 등)을 할 수 있어 CPU 리소스가 낭비되지 않는다. 
### 에러 핸들링
- 프로미스가 거부되면 throw문을 작성한 것처럼 에러가 발생한다.
    ```ts
    async function f() {
      await Promise.reject(new Error("에러 발생!"));
    }
    
    === 동일
    
    async function f() {
      throw new Error("에러 발생!");
    }
    ```
  
- await가 던진 에러는 throw가 던진 에러를 잡을 때처럼 try..catch를 사용해 잡을 수 있다. 
    ```ts
    async function f() {
    
      try {
        let response = await fetch('http://유효하지-않은-주소');
      } catch(err) {
        alert(err); 
      }
    }
    
    f();
    
    // TypeError: failed to fetch
    ```
- try..catch가 없으면 아래 예시의 async 함수 f()를 호출해 만든 프라미스가 거부 상태가 되는데, .catch를 추가해 거부된 프라미스를 처리할 수 있다.
    ```ts
    async function f() {
      let response = await fetch('http://유효하지-않은-주소');
    }
    
    // f()는 거부 상태의 프라미스
    f().catch(alert); // catch로 잡기 
    
    // TypeError: failed to fetch
    ```
    
## async/await와 promise.then/catch
- async/await을 사용하면 await가 대기를 처리해주기 때문에 .then이 거의 필요하지 않다. 
- .catch 대신 일반 try..catch를 사용할 수 있다는 장점도 있다. 
- 항상 그러한 것은 아니지만, promise.then을 사용하는 것보다 async/await를 사용하는 것이 대개는 더 편리하다. 

## async/await는 Promise.all과도 함께 사용할 수 있다. 
- 여러 개의 프라미스가 모두 처리되길 기다려야 하는 상황의 경우 Promise.all로 감싸고 여기에 await를 붙여 사용할 수 있다.
```ts
// 프라미스 처리 결과가 담긴 배열을 기다린다. 
let results = await Promise.all([
  fetch(url1),
  fetch(url2),
  ...
]);
``
