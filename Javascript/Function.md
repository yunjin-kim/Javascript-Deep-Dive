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

```js
// 함수참조
console.dir(add); // f add(x, y)
console.dir(sub); // undefined

// 함수호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TypeError: sub is not a function

// 함수선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식 
var sub = function(x, y) {
  return x - y;
}
```
자바스크립트의 모든 선언문이 선언되기 이전에 참조가 가능하다
함수 선언문으로 정의된 함수가 평가되는 과정
1. 자바스크립트 엔진이 스크립트가 로딩되는 시점에 초기화한다
2. 초기화된 변수를 변수 객체(Variable Object, VO)에 저장한다
이렇게 함수의 선언, 초기화, 할당이 한 프로세스에서 이루어진다


하지만 함수 표현식은 함수 호이스팅이 아닌 변수 호이스팅이 발상한다
1. 함수 표현식은 런타임에 해것되고 샐행되므로 변수 생성과 초기화, 할당이 분리되어 진행된다
2. 호이스팅된 변수는 undefined로 초기화된다
3. 실제로 값의 할당은 할당뭉에서 이루어진다

함수 호이스팅은 함수 호출전에 반드시 함수를 선언해야 된다는 규칙을 무시하기 때문에
코드의 구조적 결함이 발생할 수 있어 함수 표현식을 사용하는 것은 권장한다


## 함수 호출

```js
function 함수이름(매개, 변수) {   // 함
  return 매개 + 변수; // 반환값  //  수 
}                            // 정의
함수이름(인, 수); //함수 호출
```
매개변수는 함수 몸체 내부에서 변수와 동일하게 취급된다
**매개변수(Paramenters)**는 **인수(Arguments)**에 연결된다

매개변수와 인수가 1:1로 대응되지 않거나 필요한 인수를 넘겨주지 않으면
자바스크립트는 암묵적으로 매개변수가 생성되고 undefined로 초기화되어
함수 호출 시 입력한 인수의 순서대로 할당되는 형태이다

- 함수는 매개변수와 인수의 개수가 일치하는지 체크하지 않는다
```js
function sum(a, b) {
  console.log(a, b);    //undefined
  return a + b;
}

console.log(sum(1));  // NaN
```

- 인수가 매개변수보다 많은 경우 에러없이 무시된다
```js
function sum(a, b) {
  return a + b;
}

console.log(sum(1, 2, 3)); // 3
```

- 초과한 인수가 사라지는 것은 아니다. arguments라는 객체에 프로퍼트로 보관된다
```js
function sum(a, b {console.log(aruments)}) { }
                  // Arguments(3) [1, 2, 3, caljames: ƒ, Symbol(Symbol.iterator): ƒ]
sum(1, 2, 3);
```
arguments 객체는 가변 인자 함수를 구현할 때 유용하ㅏㄷ

- 자바스크립트는 매개변수 타입을 사전에 지정할 수 없다

- 매개변수의 개수는 제한이 없다. 객체 형태로 관리되고 있기 때문
  매개변수의 개수는 최소한으로 해야한다
  많은 매개변수를 넘겨야 하는 상황이라면 객체 형태로 전달한다
  - 객체를 인수로 사용하면 참조 값을 바라보게 되므로 원본 객체에 변경이 가해지니 주의해야 한다
```js
function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = 'Hong';
  obj.age = 33;
  console.log(primitive); // 200
}

const num = 100;
const obj = {
  name: 'Lee',
  age: 22
};

console.log(num); // 100
console.log(obj); // {name: 'Lee', age: 22}

changeVal(num, obj);

console.log(num); // 100
console.log(obj); // {name: 'Lee', age: 33}
```
**원시값 num**은 변경이 불가능하므로 재할당을 통해 새로운 원시값으로 교체한다
**객체(obj)**는 변경이 가능하므로 재할당없이 직접 값을 교체한다
원시값을 매개변수로 전달하면 원본 값을 훼손하지 않아 부수 효과가 일어나지 않고
객체는 일어난다
이를 해결하기 위해 객체를 불변 객체로 만들거나 깊은 복사하는 방법이 있다


## 순수 함수, 비순수 함수

순수 함수: 순수 함수가 반환한 결과가 변수에 재할당하므로 추적이 쉽다
```js
let count = 0;
// 순수 함수
function increase(n) {
  return ++n;
}

count = increase(count);
console.log(count); // 1
count = increase(count);
console.log(count); // 2
```

비순수 함수: 외부 상태를 변경해 상태변화를 추적하기 힘들다
```js
let count = 0;
// 비순수 함수
function increase() {
  return ++count;
}

increase();
console.log(count); // 1
increase();
console.log(count); // 2
```

순수 함수는 부수 효과를 최대한 억제하여 오류를 피해 프로그램의 안정성을 높인다


## 반환문

함수 호출 표현식은 return 키워드가 반환한 반환값으로 평가된다

반환문의 역할
- 함수의 실행을 중단하고 외부로 결과값을 반환한다
```js
function sum(a, b) {
  return a + b;

  console.log(a, b); // 실행 X
}
console.log(sum(1, 2)) // 3
```

- 반환문을 지정하지 않거나 생략하면 undefined를 반환한다
```js
function foo() {
  retrun;
}
function bar() {

}
console.log(foo()); // undefined
console.log(bar()); // undefined
```

- return 키워드를 사용할 때 줄 바꿈을 한다면 ASI(세미콜론 자동 삽입 기능)에 의해 undefined가 반환된다
```js
function sum(a, b) {
  return
    a + b;
}
/* 
  function sum(a, b) {
  return;
    a + b;
}
*/
console.log(sum(1, 2)); // undefined
```

- 반환문은 함수의 몸체에서만 사용할 수 있다


## 즉시 실행 함수

- 함수의 선언과 동시에 즉시 호출되는 함수로 다시 호출할 수 없다

- 기명 함수로 작성해도 상관없으나 다시 사용할 수 없으므로 익명 함수로 작성하는 것이 일반적이다

- 반드시 그룹연산자**( () )**로 감싸야 한다. 그 이유는 함수 선언문의 형식과 일치하지 않기 때문이며 ASI 기능 또한 에러를 발생시킨다
```js
(function() {

}());

const res = (function() {
  let a = 3;
  let b = 3;
  return a + b;
}());

console.log(res); // 6

const sum = (function(a, b) {
  return a + b; 
}(3, 5));

console.log(sum); // 8
```


## 재귀 함수

함수가 자기 자신을 호출하는 것을 재귀 호출이라 한다
재귀 함수는 재귀 호출을 수행하는 함수를 말한다

- 재귀 함수는 자신을 무한으로 호출하므로 반드시 탈출 조건을 명시해야 한다
```js
function counter(n) {
  if (n > 10) return;
  counter(n + 1); // 재귀 호출
}

counter(1);
```
재귀 함수는 무한 반복에 빠질 위험이 있기 때문에 반복문을 사용하는 것보다
재귀 함수를 사용하는 편이 직관적으로 이해하기 쉬울 때 한정적으로 사용한다


## 중첩함수

함수 내부에 정의된 함수를 중첩함수, 내부 함수라 한다 .중첩 함수를 포함하는 함수는 외부 함수하고 부른다. 중첩 함수는 외부 함수 내부에서만 호출할 수 있다
중첩함수는 외부함수를 돕는 헬퍼 함수 역할을 한다
```js
function outer() {
  let x = 1;
  // 중첩 함수
  function inner() {
    let y = 2;
    console.log(x + y); // 3
  }
  inner();
}

outer();
```


## 콜백 함수

함수의 매개변수로 내부에 전달되는 함수를 콜백 함수라고 하며
이를 전달 받은 함수를 고차 함수라고 한다
```js
// 고차 함수
function repeat(n, f) {
  for (let i = 0; i < n; i++) {
    f(i); // 콜백 함수 f()호출
  }
}
```
- 내부 함수가 외부 함수를 돕는 것과 같이 콜백 함수도 고차 함수에 전달되어 돕는 역할을 한다
- 콜백 함수는 함수 외부에서 주입되므로 내부 하무와 달리 자유롭게 교체할 수 있다. 고차 함수는 콜백 함수를 자신의 일부분인 마냥 합성한다
- 고차 함수는 매개변수를 통해 전달 받은 콜백 함수의 호출 시점을 결정해서 호출하는데 이 때 콜백 함수에 인수를 전달할 수 있다
- 고차 함수 내부에서만 호출되는 경우 콜백 함수를 익명 함수 리터럴로 정의하는 것이 일반적이다
```js
// 익명 함수 리터럴을 콜백 함수로 고차 함수에 전달한다
foo(5, function(i) {
  console.log(i);
});
```
```js
const foo = function(i) {
  if (i % 2) console.log(i);
};
foo(5, foo);
```