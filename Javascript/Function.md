# 함수

함수는 일련의 과정을 문으로 구현하고 코드 블럭으로 감싸서 하나의 실행 단위로 정의한 것이다

### 함수의 기능
- 반복적으로 작성되는 코드를 함수로 정의하여 재사용
- 객체 생성
- 객체의 행위 정의(메서드로 동작)
- 정보 은닉
- 클로저
- 모듈화

자바스크립트의 함수는 **일급 객체**다
변수에 할당할 수 있고 프로퍼티의 값이 될 수 있으며 배열의 요소가 될 수 있다


## 일급 객체

일급 객체의 조건
- 무명 리터럴로 생성(런타임에 생성 가능) 할 수 있다
- 변수, 각종 자료구조(Object, Array, Set, Map) 에 저장할 수 있다
- 함수의 매개변수로 전달할 수 있다
- 함수의 반환값으로 사용할 수 있다
```js
// 1, 2번
const increase = function(num) {
  return ++num;
}
console.log(increase);  // f (num) { return ++num }
console.log(increase(3)); // 4

// 3번
const predicate = { increase };
console.log(predicate); // { increase: f }
console.log(predicate.increase(3)); // 4

// 3, 4번
function counter(predicate) {
  let num = 0;

  return function() {
    num = predicate(num);
    return num;
  };
}

const increaser = counter(predicate.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2
```

함수가 일급 객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미이다
햠수는 값을 가용할 수 있는 곳이라면 리터럴로 정의할 수 있고 런타임에 함수 객체로 퍙가되는 것이다


## 함수 정의

1. 함수 선언문(Function Declaration): function 키워드와 함수명, 매개변수 목록, 함수 몸체로 구성된다
```js
function sum(a, b) {
  return a + b;
}

console.log(sum(1, 2)) // 3
```
- 함수 선언문은 함수명을 생략할 수 없다
- 매개변수를 넘기지 않으면 undefined로 선언된다
- 함수 몸체는 함수 호출 시 실행되는 문들의 집합이다. 중괄호로 감싸지며 return 문으로 함수 실행결과를 반환할 수 있다


2. 함수 표현식(Function Exporession): 함수 리터럴 방식으로 함수를 정의하고 변수에 할당한다

- 기명 함수 표현식: 함수명을 지정하는 방법
```js
const foo = function sum(a,b) {
  return a + b;
}
console.log(foo(1, 2)); // 3
console.log(sum(1, 2)) // ReferenceError: sum is not defined
```
  - 이처럼 변수에 할당할 수 있는 이유는 함수가 일급 객체로 평가되기 때문이다. 해당 변수는 원시값이 아니라 할당된 함수를 가리키는 참조값을 저장하며, 함수명이 아닌 변수명으로 함수를 호출할 수 있다

- 익명 함수 표현식: 함수명을 생략하는 방법
```js
const bar = function(a, b) {
  return a + b;
}
const foo = bar;

console.log(bar(1, 2)); // 3
console.log(foo(1, 2)); // 3
```

3. Function 생성자 함수: 사용 XXXX

4. 화살표 함수: function 키워드 대신 화살표를 사용한다. 항상 익명 함수로 정의한다
```js
const bar = (a, b) => {
  return a + b;
}

console.log(bar(1, 2)); // 3
```


## 함수 호이스팅