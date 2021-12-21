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