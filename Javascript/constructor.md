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