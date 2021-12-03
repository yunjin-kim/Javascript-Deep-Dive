## 클래스

자바스크립트는 프로토타입 기반 객체지향 언어이다
프로토타입 기반 객체지향 언어는 클래스가 필요없는 객체지향 프로그래밍 언어다
클래스 없이도 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다

```js
// 생성자 함수
var Person = (function () {
  // 생성자 함수, Constructor
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log('Hola' + this.name);
  };

  // 생성자 함수 반환
  return Person;
}());

// 인스턴스 생성
var me = new Person('Kim');
me.sayHi(); // Hola kim

console.log(me instanceof Person); // true
```

```js
// 클래스 선언문 
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi ${this.name}`);
  }

  // 정적 메서드
  static sayHola() {
    console.log('Hola');
  }
}

// 인스턴스 생성
const me = new Person('Kim');
// 인스턴스의 프로퍼티 참조
console.log(me.name); // Kim
// 프로토타입 메서드 호출
me.sayHi();
// 정적 메서드 호출
Person.sayHola(); //Hola

```

### 클래스와 생성자 함수의 차이
- 클래스
  - 클래스를 new 연산자 없이 호출하면 에러 발생한다 
  - 클래스는 상속을 지원하는 extends, super 키워드를 제공한다
  - 클래스는 호이스팅이 발생하지 않은 것처럼 동작한다
  - 클래스는 constructor, 프로토타입 매서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false이다, 열거되지 않는다
- 생성자 함수
  - 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출한다
  - 생성자 함수는 extends, super 키워드를 지원하지 않는다
  - 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅, 함수 표현식으로 정의된 생성자 함수는 변수 호이스팅이 발생한다
    - 함수 선언문 function 함수명() {}, 함수 표현식 const 함수명 = function () {}


## 클래스 정의
클래스는 class 키워드를 사용해서 정의한다, 클래스 이름은 파스칼 케이스를 사용하는 것이 일반적이다
```js
// 클래스 선언문
class Person {}

// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```
클래스를 표현식으로 정의할 수 있다는 것은 클래스가 값으로 사용될 수 있는 일급 객체라는 뜻이다
클래스는 일급 객체로 다음과 같은 특징을 갖는다
- 무명의 리터럴로 생성할 수 있다, 즉 런타임에 생성이 가능하다
  - 리터럴은 변수 및 상수에 저장되는 값 자체를 일컫는 말이다
  - 변수와 상순는 메모리에 할당된 '공간' 이라면, 리터럴은 공간에 저장하는 '값' 이다
- 변수나 자료구조(객체, 배열 등)에 저장할 수 있다
- 함수의 매개변수에게 전달할 수 있다
- 함수 반환값으로 사용할 수 있다

클래스는 함수다
따라서 클래스는 값처럼 사용할 수 있는 일급 객체이다
클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드가 있다


## 클래스 호이스팅