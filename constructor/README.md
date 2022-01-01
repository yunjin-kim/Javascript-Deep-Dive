## 생성자 함수에 의한 객체 생성

### Object 생성자 함수

new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다
그 후 프로퍼티나 메서드를 추가하여 객체를 완성할 수 있다
```js
// 빈 객체 생성
const person = new Object();
// 프로퍼티 추가
person.name = 'Hong';

console.log(person); // { name: 'Hong' }
```
new 연산자를 사용하여 객체를 생성하는 함수가 생성자 함수(Constructor)이며
자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, 
Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공한다
이 생성자 함수로 생성된 객체를 인스턴스라고 한다


### 생성자 함수

- 객체 리터럴에 의한 객체 생성 방식
```js
const circle1 = {
  radius: 5,
  getDiamete() {
    return 2 * this.radius;
  }
};

console.log(circle1.getDiameter()); // 10

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle2.getDiameter()); // 20
```
- {} 로 객체를 만들면 직관적이고 간편하지만 단 하나의 객체만 생성하므로
  동일한 프로퍼티를 갖는 객체를 여러 개 생성하야 하는 경우 비효율적이다

- 생성자 함수에 의한 객체 생성 방식
```js
function Circle(radius) {
  this.radius = radius;
  this.getD = function () {
    return 2 * this.radius;
  }
}

const circle1 = new Circle(5);
const circle2 = new Circle(6);
```
- 객체(인스턴스)를 생성하기 위한 템플릿(클래스)차람 생성자 함수를
  사용하여 프로퍼티 구조가 동일한 객체 여러개를 생성할 수 있습니다

생성자 함수는 new 연산자와 함께 호출해야 생성자 함수로 동작한다
new 연산자를 호출하지 않으면 일반 함수로 동작한다

this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다
this가 가리키는값 즉 this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다
```js
function foo() {
  console.log(this);
}

// 일반 함수로 호출
// 전역 객체는 브라우저 환경에서는 window, Node.js 환경에서는 global을 기리킨다
foo(); // window

const obj = { foo };
// 메서드로서 호출
obj.foo(); // obj

//생성자 함수로서 호출
const inst = new foo(); // inst
```

### 생성자 함수의 인스턴스 생성 과정

생성자 함수가 인스턴스를 생성하는 것은 필수이고, 생성된 인스턴스를 초기화하는 것은 옵션이다

자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다
new 연산자와 함꼐 생성자 호출하면  아래와 같이 흐른다

1. 인스턴스 생성과 this 바인딩
  - 런타임 이전에 암묵적으로 빈 객체를 생성(완성되지는 않았지만 이 빈 객체가 생성자 함수로 
    생성한 인스턴스)한 뒤 this에 바인딩한다

2. 인스턴스 초기화
  - 생성자 함수에 기술된 코드가 실행되어 this에 바인딩된 인스턴스를 초기화한다
  - 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수와 인수로 전달받은
    초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값에 할당한다

3. 인스턴스 반환
  - 생성자 함수 내부의 모든 처리를 끝내고 완성된 인스턴스에 바인딩 된 this가 암묵적으로 반환된다
  - 이때 return에 객체를 반환하도록 명시되어 있다면 암묵적인 this 반환이 무시되므로 생략하거나
    return this를 사용한다

```js
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩
  console.log(this); // Circle {}

  // 2. this에 바인딩 된 인스턴스 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 암묵적으로 this 반환
  // 명시적으로 객체를 반환하면 암묵적으로 this 반환이 무시된다
  return {};

  // 원시값 반환시 무시되고 암묵적으로 this가 반환된다
  return 100;
}

const circle= new Circle(1);
console.log(circle); // Circle { radius: 1, getDiameter: [Function (anonymous)] }
```


## 내부 메서드 [[Call]] 과 [[Construct]]

함수는 객체이지만 일반 객체와는 달리 호출할 수 있다
그 이유는 함수 객체는 일반 객체의 내부 슬롯과 내부 메서드를 포함하여 함수로서 동작하기 위한
내부 슬롯 ([[Environment]], [[FormalParameters]] 등)과 내부 메서드([[Call]], [[Construct]])를 추가로 가지고 있다
```js
function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출된다
foo();

// 생성자 함수로서 호출: [[Constructor]]가 호출된다
new foo();
```
- 일반 함수로서 호출: 함수 객체 내부 메서드인 [[Call]] 호출
  - callable 이라고 부른다
- 생성자 함수로서 호출: 내부 메서드 [[Constructor]] 호출
  - constructor 라고 부른다
  - [[Constructor]]가 없는 함수 객체를 non-constructor라고 부른다

함수로서 기능하는 개체는 반드시 callable이 되어야 한다
따라서 함수 객체는 내부 메서드 [[Call]]을 갖고 있으므로 호출이 가능하다
하지만 모든 함수 객체가 [[Constructor]]를 갖는 것은 아니다


### constructor 과 non-constructor의 구분

자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 정의 방식에 따라 구분한다
- constructor: 함수 선언문, 함수 표현식, 클래스
- non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수

```js
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const fuc = function () {};
// 프로퍼티 x의 값으로 할당된 것은 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다
const bar = {
  x: function () {}
}

// 일반 함수로만 정의된 함수만이 constructor다
new foo(); // -> foo {}
new fuc(); // -> bar {}
new bar.x(); // -> x {}

// 화살표 함수로 정의 
const arrow = () => {};
new arrow (); // TypeError: arrow is not a constructor

// 메서드 정의: ES6의 메서드 축약 표현만 메서드로 인정한다
const obj = {
  x() {}
};

new obj.x(); // TypeError: obj.x is not a constructor
```

### new 연산자

new 연산자와 함께 함수를 호출하면 함수 객체의 내부 메서드 [[Constructor]]가 호출된다
new 연산자와 함께 호출되는 함수는 constructor이어야 한다

생성자 함수가 new 연산자 없이 호출된다면 에러가 발생한다
이것을 방지하기 위해서 ES6 에서는 new.target을 지원한다

#### new.target

new.target은 this와 유사하게 constructor인 모든 함수 내부에서 
암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다

함수 내부에서 new.target을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다
new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다
new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined이다

따라서 함수 내부에서 new.target을 사용하여 new 연산자와 생성자 함수로서 호출했는지 확인하여 
그렇게 않은 경우 new 연산자와 함께 재귀 호출을 통해 생성자 함수로서 호출할 수 있다
```js
// 생성자 함수
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다
  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성한 인스턴스를 반환한ㄷ
    return new Circle(radius);
  }

  // new를 호출하지 않으면 this가 다른 객체에 바인딩 되는 것을 이용한다
  // ES6 new.target을 지원하지 않는 브라우저를 위한 스코프 세이프 생성자 패턴이다
  if (!(this instanceof Circle)) {
    return new Circle(radius);
  } 
}
```