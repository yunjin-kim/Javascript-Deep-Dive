# 클로저

클로저는 함수를 일급 객체로 취급하는 함수형 프로그래밍 언어에서 사용되는 중요한 개념이다

**클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다**

```js
const x = 1;

function outer() {
  const x = 10;
  
  function inner() {
    console.log(x); // 10
  }
  inner();
}
outer();
```
중첩 함수 inner의 상위 스코프는 외부 함수 outer의 스코프이다
따라서 중첩 함수 inner 내부에서 자신을 포함하고 있는 외부 함수 outer의 x 변수에 접근할 수 있다

만약 inner 함수가 outer 함수의 내부에서 정의된 중첩 함수가 아니라면 inner 함수를
outer 함수의 내부에서 호출하더라도 outer 함수에 접근할 수 없다
```js
const x = 1;

function outer() {
  const x = 10;
  inner();
}

function inner() {
  console.log(x); // 1
}

outer();
```
이 같은 현상이 발생하는 이유는 자바스크립트가 렉시컬 스코프를 따라는 프로그래밍 언어이기 때문이다


## 렉시컬 스코프

**자바스크립트 엔진은 함수를 어디서 호출했는지 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다. 이를 렉시컬 스코프(정적 스코프)라 한다**

```js
const x = 1;

function foo() {
  const x = 10;
  bar();
}

function bar() {
  console.log(x); // 1
}

foo();
bar();
```
foo 함수 bar 함수 모두 전역에서 정의되었다
함수를 어디서 호출하는지는 함수의 상위 스코프 결정에 어떠한 영향도 주지 못한다
함수의 상위 스코프는 함수를 정의한 위치에 의해 정적으로 결정되고 변하지 않는다

스코프의 실체는 실행 컨텍스트의 렉시컬 환경이다
이 렉시컬 환경은 자신의 외부 렉시컬 환경에 대한 참조를 통해 상위 렉시컬 환경과 연결된다
이것이 바로 스코프 체인이다

**함수의 상위 스코프를 결정한다**라는 것은 **렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값을 결정한다**는 것과 같다
렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값이 바로 상위 렉시컬 환경에 대한 참조이며
이것이 상위 스코프이기 때문이다
**렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다. 이것이 바로 렉시컬 스코프이다**


## 함수 객체의 내부 슬롯[[Environment]]

함수는 자신의 내부 슬롯 **[[Environment]]**에 자신이 정의된 환경인 상위 스코프의 참조를 저장한다

함수가 정의된 환경(위치)과 호출되는 환경(위치)은 다를 수 있다
따라서 렉시컬 스코프는 자신의 정의된 환경에서의 상위 스코프를 기억하는데
이 때 자신의 내부 슬롯인 **[[Environment]]**에 상위 스코프의 참조를 저장하고
이는 실행 중인 실행 컨텍스트의 렉시컬 환경을 가리키게 된다

그 이유는 함수 정의가 평가되어 함수 객체를 생성하는 시점은 상위 함수(전역 코드)가 평가되고 
실행되는 시점이고 실행 컨테스트는 상위 함수(전역 코드)이기 때문이다
결국 함수 객체의 내부 슬롯 **[[Environment]]**에 저장된 현재 실행 중인 실행 컨텍스트의
렉시컬 환경의 참조가 바로 상위 스코프가 되는 것이다

함수가 호출되면 함수 내부로 코드 제어권이 이동하고 함수는 애래 순서로 진행된다
1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성
  2-1 함수 환경 레코드 생성
  2-2 this 바인딩
  2.3 외부 렉시컬 환경에 대한 참조 결정

외부 렉시컬 환경에 대한 참조에는 함수 객체 내부 슬롯 **[[Environment]]**에 저장된 렉시컬 환경의 참조가 할당되고, 이는 함수의 상위 스코프를 의미한다
이것이 바로 함수 정의 위치에 따라 상위 스코프를 결정하는 렉시컬 스코프이다


## 클로저와 렉시컬 환경

외부 함수보다 더 오래 유지되어 외부 함수의 변수를 참조할 수 있는 내부 함수를 클로저라고 부른다

```js
const x = 1;

function outer() {
  const x = 10;
  const inner = function () { console.log(x) };
  return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다
const innerFunc = outer();
innerFunc(); // 10
```
- outer 함수를 호출하면 outer 함수는 중첩 함수 inner를 반환하고 outer 함수는 종료되어 실행 컨텍스트 스택에서 제거(pop)된다
- outer 함수의 실행 컨텍스트가 제거되었으므로 outer 함수의 지역 벼누 x 또한 생명 주기를 마감한다
- outer 함수의 지역 변수 x는 더 이상 유효하지 않게 되어 실행 결과는 1로 예상된다

그러나 실행결과는 outer 함수의 지역 변수 x의 값인 **10**이 출력된다
이처럼 **외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료된 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 클로저라고 부른다**

outer 함수의 실행이 종료되어 실행 컨텍스트 스택이 제거된다고 outer 함수의 렉시컬 환경까지 소멸되는 것은 아니다
자바스크립트 엔진은 [표시하고-쓸기(Mark-and-sweep) 알고리즘](https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory_Management#mark-and-sweep_algorithm)를 사용한 가비지 콜렉터를 사용하고 있기 때문이다

outer 함수의 렉시컬 환경은 현재 반환된 익명 함수의 내부 슬롯 **[[Environment]]**이 참조하고 있으며
이는 전역 변수 innerFunc에 할당했으므로 가비지 콜렉터의 대상이 되지 않는 것이다
가비지 콜렉터는 참조되고 있는 메모리 공간을 제거하지 않는다


자바스크립트의 모든 함수는 상위 스코프를 기억하므로 모든 함수는 클로저다
하지만 일반적으로 모든 함수를 클로저라고 하지 않는다
```js
function foo() {
  const x = 1;
  
  function bar() {
    const y = 2;
  }

  return bar;
}

const bar = foo();
bar();
```
중첩 함수 bar는 외부 함수 foo보다 더 오래 유지되지만 상위 스코프의 어떤 식별자도 참조하지 않는다
이처럼 상위 스코프의 어떤 식별자도 참조하지 않는 경우 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 않는다
참조하지도 않는 식별자를 기억하는 것은 메모리 낭비이기 때문이다
따라서 bar 함수는 클로저라고 할 수 없다


## 클로저 활용

**클로저는 상태(state)를 안전하게 변경하고 유지하기 위해 사용한다**
**상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다**
```js
// 함수를 인수로 전달받고 함수를 변환하는 고차 함수
// 자유 변수 counter를 기억하는 클로저를 반환한다
function makeCounter(predicate) {
  // 카운터 상태 변수
  let counter = 0;

  // 클로저 반환
  return function() {
    // 인수로 전달 받은 보조 함수에 상태 변경 위임
    counter = predicate(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 함수로 험수를 생성한다
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수를 반환한다
const increaser = makeCounter(increase);
console.log(increase()); // 1
console.log(increase()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 땨문에 카운터 상태가 연동되지 않는다
const decreaser = makeCounter(decrease);
console.log(decrease()); // -1
console.log(decrease()); // -2
```
makeCounter 함수는 고차함수이다
mkaeCounter 함수가 반환하는 함수는 자신이 생성되었을 때의 렉시컬 환경인 makeCounter 함수의 스코프에 속한 counter 변수를 기억하는 클로저다
makeCounter 함수는 인자로 전달받은 보조 함수를 합성하여 자신이 반환하는 함수의 동작을 변경할 수 있다
**makeCounter 함수를 호출해 함수를 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다**

makeCounter 함수를 호출하면 makeCounter 함수의 실행 컨텍스트가 생성된다
그리고 makeCounter 함수는 함수 객체를 생성하여 반환한 후 소멸된다
makeCounter 함수가 반환한 함수는 makeCounter 함수의 렉시컬 환경을 상위 스코프로 기억하는 클로저이며 전역 변수인 increaser에 할당된다
이 때 makeCounter 함수의 실행 컨텍스트는 소멸되지만 makeCounter 함수의 실행 컨텍스트의 렉시컬 환경은 makeCounter 함수가 반환한 함수의 **[[Environment]]** 내부 슬롯에 의해 참조되고 있기 때문에 소멸되지 않는다


독립된 counter가 아니라 연동하여 증감이 가능한 카운터를 만들려면 렉시컬 환경을 공유하는 클로저를 만들어야 한다
```js
// 함수를 인수로 전달받고 함수를 변환하는 고차 함수
// 자유 변수 counter를 기억하는 클로저를 반환한다
const counter = (function() {
  // 카운터 상태 변수
  let counter = 0;
  
    // 클로저 반환
  return function() {
    // 인수로 전달 받은 보조 함수에 상태 변경 위임
    counter = predicate(counter);
    return counter;
  };
}());

// 보조 함수 
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 보조함수를 전달 받아 호출
console.log(counter(increase)); // 1
console.log(counter(increase)); // 2

// 자유 변수를 공유한다
console.log(counter(decrease)); // 1
console.log(counter(decrease)); // 0
```


## 정리
렉시컬 스코프는 정적 스코프라고 부를 수 있다. 정적 스코프는 어디에서 정의했는지에 따라 상위 스코프가 결정된다
클로저는 중첩 함수에서 내부 함수가 만들어지는 시점에서 내부 함수의 부모 함수가 가지고 있는 유효범위, 변수 등을 내부 함수가 동봉(closure)해서 가지고 있다
그래서 언제든지 호출하면 변수 등에 접근할 수 있다

[생활코딩](https://www.youtube.com/watch?v=bwwaSwf7vkE)