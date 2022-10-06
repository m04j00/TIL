## Promise

> 비동기 작업의 단위이다.
> 

```jsx
const promise = new Promise((resolve, reject) => {
	resolve();
});
promise
	.then(() => {
		console.log('then!');
	});
	.catch(() => {
		console.log('catch!');
	});

// then!
```

1. `const promise` 하나의 변수로 끝까지 해당 Promise를 관리하는 게 가독성이 좋고 유지보수도 하기 좋다. 
2. `new Promise` Promise 객체 생성
3. `(resolve, reject) => {}` executor
    - `resolve` executor 내에서 호출할 수 있는 도 다른 함수이다. resolve 호출 시 비동기 작업이 성공했다는 의미이다.
    - `reject` executor 내에서 호출할 수 있는 도 다른 함수이다. reject 호출 시 비동기 작업이 실패했다는 의미이다.

- Promise를 선언하는 순간 할당된 비동기 작업이 바로 시작된다.
- Promise가 끝난 뒤 `then()` 과 `catch()`로 다음 동작을 설정한다.
    - `then` Promise가 성공했을 때의 동작을 지정한다. 인자로 함수를 받는다.
    - `catch` Promise가 실패했을 때의 동작을 지정한다. 인자로 함수를 받는다.

- 대기
- 이행
- 거부

### 의의

비동기 작업을 생성 / 시작하는 부분(`new Promise()`)과 작업 이후의 동작 지정 부분(`then`, `catch`)을 분리함으로써 기존의 러프한 비동기 작업보다 유연한 설계를 가능하도록 한다. 

## async

> 비동기 작업을 만드는 손쉬운 방법
> 

```jsx
async function startAsync(age){
	if(age > 20) return `${age} success`;
	else throw new Error(`${age} is not over 20`);
}

const promise = startAsync(25);
promise
	.then((value) => {
		console.log(value);
	})
	.catch((err) => {
		console.error(err);
	});
```

- 함수에 `async` 키워드를 붙인다.
- `new Promise` 부분을 없애고 `executor` 본문 내용만 남긴다.
- `resolve(value)` 부분을 `return value;` 로 변경한다.
- `reject(new Error())` 부분을 `throw new Error()` 로 변경한다.

- `async` 함수의 반환 값은 무조건 `Promise`이다.

## await

> Promise가 끝날 때까지 기다리기
> 

```jsx
async function startAsyncJobs(){
	await setTimeoutPromise(1000);
	const promise = startAsync(25);
	try{
		const value = await promise;
		console.log(value);
	} catch(e){
		console.log(e);
	}
}
```

- 문법적으로 `await [[Promise 객제]]` 로 사용한다.
- await은 Promise가 완료될 때까지 기다린다.  `setTimeoutPromise` 의 executor에서 resolve 함수가 호출될 때까지 기다린다. 기다리는 시간동안 `startAsyncJobs` 의 진행은 멈춰있다.
- await은 Promise가 resolve한 값을 내놓는다.
- 해당 Promise에서 reject 발생 시 예외가 발생한다. 예외 처리를 위해 `try-catch`구문을 사용한다.

- 실제 작업이 끝나는 걸 기다린 다음, 다음 코드를 수행한다.

## 참고

[https://elvanov.com/2597](https://elvanov.com/2597)
