# 비구조화 할당

- 객체 안에 있는 값을 추출해서 변수 혹은 상수로 바로 선언할 수 있다.

```js
const object = {a: 1, b: 2};
const {a, b} = object;

console.log(a, b); // 1 2
```

```js
const object = {a: 1};

function print({a, b}) {
    console.log(a, b);
}

print(object); // 1 undefined
```

## 기본값 설정

```js
const object = {a: 1};

const {a, b = 2} = object;

function print({a, b = 2}) {
    console.log(a, b);
}

console.log(a, b); // 1, 2
print(object); // 1, 2
```

## 변수명(Key) 변경하기

```js
const object = {a: 1};

const {a: a1} = object;

console.log(a1); // 1
```

## 배열 비구조화 할당

```js
const array = [1, 2];

const [a, b, c = 3] = array;

console.log(a, b, c); // 1 2 3 
```

## 깂은 값 비구조화 할당

```js
const deepObject = {
  state: {
    information: {
      name: 'velopert',
      languages: ['korean', 'english', 'chinese']
    }
  },
  value: 5
};

// 1. 비구조화 할당 문법 2번 사용하기 
const {name, languages} = deepObject.state.information;
const {value} = deepObject;

const extracted = {
    name, 
    languages,
    value
};

// 2. 한번에 모두 추출
const {state: {information:{name, languages}}, value} = deepObject;

const extracted2 = {
  name,
  languages,
  value
};
```

### ES6 의 object-shorthand 문법

- key 이름으로 선언된 값이 있으면 바로 value로 매칭해주는 문법

```js
const extracted = {
  name,
  languages,
  value
}

== 

const extracted = {
  name: name,
  languages: languages,
  value: value
}
```
