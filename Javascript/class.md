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
  - 변수와 상수는 메모리에 할당된 '공간' 이라면, 리터럴은 공간에 저장하는 '값' 이다
- 변수나 자료구조(객체, 배열 등)에 저장할 수 있다
- 함수의 매개변수에게 전달할 수 있다
- 함수 반환값으로 사용할 수 있다

클래스는 함수다
따라서 클래스는 값처럼 사용할 수 있는 일급 객체이다
클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드가 있다


## 클래스 호이스팅

클래슨는 함수로 평가된다

```js
// 클래스 선언문
class Person {}

console.log(typeof Person); // function
```

클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가과정, 즉 런타임 이전에 평가되어 함수 객체를 생성한다
이때 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 constructor 이다
생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다
프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다

클래스는 클래스 정의 이전에 참조할 수 없다
```js
console.log(Person);
// ReferenceError: Cannot access 'Person' before initialization

// 클래스 선언문 
class Person {}
```

클래스 선언문은 호이스팅이 발생하지 않는 것처럼 보이나 그렇지 않다
```js
const Person = '';
{
  //호이스팅이 발생하지 않으면 ''가 출력되어야 한다
  console.log(Person);
  // ReferenceError: Cannot access 'Person' before initialization

  // 클래스 선언문
  class Person {}
}
```
클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생한다
단, 클래스는 let, const 키워드로 선언한 변수처럼 호이스팅된다
따라서 클래스 선언문 이전에 일시적 사각지대에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다

var, let, const, function, function*, class 키워드를 사용하여 선언된 모든 식별자는 호이스팅된다
모든 선언문은 타임 이전에 먼저 실행된다


## 인스턴스 생성

클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스를 생성한다
```js
class Person {}

//인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```

함수는 new 연산자의 사용 여부에 따라 일반 함수로 호출되거나 인스턴스 생성을 위한 생성자 함수로 호출되지만
클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 new 연산자와 함꼐 호출한다
```js
const Person = class Develop {};
// 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다
const me = new Develop();
//클래스 이름 Develop은 함수와 마찬가지로 클래스 몸체 내부에서만 유효한 식별자다
console.log(Develop); // ReferenceError: Develop is nit defined

const mine = new Develop(); // ReferenceError: Develop is nit defined
```


## 메서드

클래스 몸체에 정의할 수 있는 메서드는 constructor, 프로토타입 메서드, 정적 메서드가 있다

### constructor
constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다
```js
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

클래스는 평가되어 함수 객체가 된다
함수와 동일하게 프로토타입과 연결되어 있으며 자신의 스코프 체인을 구성한다

모든 함수객체가 가지고 있는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리키고 있따
이는 클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미한다
new 연산자와 함꼐 클래스를 호출하면 클래스는 인스턴스를 생성한다

생성자 함수와 마찬가지로 constructor 내부에서 this에 추가한 프로터피는 인스턴스 프로퍼티가 된다
constructor 내부의 this는 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스를 가리킨다

constructor는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다

프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다
```js
class Person {
  constructor() {
    // 고정값으로 인스턴스 초기화
    this.name = 'Hong';
    this.age = 20;
  }
}
// 인스턴스 프로퍼티가 추가된다
const me = new Person();
console.log(me); //Person {name: "Hong", age: 20}
```

인스턴스를 생성할 때 클래스 외부에서 인스턴스 프로터티의 초기값을 전달하려면 constructor에 매개변수를 선언하고
인스턴스를 생성할 때 초기값을 전달한다
이때 초기값은 constructor의 매개변수에게 전달된다
```js
class Person {
  constructor(name, age) {
    // 인수로 인스턴스 초기화
    this.name = name;
    this.age = age;
  }
}
// 인수로 초기값을 전달한다, 초기값은 constructor에 전달된다
const me = new Person("Hong", 20);
console.log(me); // Person {name: "Hong", age: 20}
```

constructor는 별도의 반환문을 갖지 않아야 한다
new 연산자와 함께 클래스를 호출하면 생성자 함수와 동일하게 암묵적으로 this, 즉 인스턴스를 반환하기 때문이다
```js
class Person {
  constructor(name) {
    this.name = name;
  // 명시적으로 객체를 반환하면 this 반환이 무시된다
    retunr {}
  }
}
// constructor에서 명시적으로 반환한 빈 객체가 반환된다
const me = new Person('Hong');
console.log(me); // {}
```

명시적으로 원시값을 반환하면 원시값 반환은 무시되고 this가 반환된다


### 프로토타입 메서드

클래스 몸체에서 정의한 메서드는 생성자 함수와는 다르게 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다
```js
class Person {
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
  // 프로토타입 메서드
  sayHi() {
    console.log(`Hola ${this.name}`);
  }
}
const me = new Person('Hong');
me.sayHi(); // Hola Hong
```
생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다


### 정적 메서드

정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다
클래스에서는 static 키워드를 붙여주면 정적 메서드가 된다
```js
class Person {
  constructor(name) {
    this.name = name;
  }
  // 정적 메서드
  static sayHi() {
    console.log('Hi');
  }
}
```
정적 메서드는 클래스에 바인당된 메서드가 된다
클래스는 함수 객체로 평가되므로 자신의 프로퍼티/메서드를 소유할 수 있다
클래스는 클래스 정의(클래스 선언문, 클래스 표현식)가 평가되는 시정에 함수 객체가 되므로
인스턴스와 달리 별다른 생성과정이 필요없다
따라서 정적 메서드는 클래스 정의 이후 인스턴스를 생성하지 않아도 호출할 수 있다
```js
// 정적 메서드는 클래스로 호출된다
// 정적 메서드는 인스턴스 없이도 호출할 수 있다
Person().sayHi();
```
정적 메서드는 인스턴스로 호출할 수 없다
정적 메서드는 클래스에 속하고 클래스는 인스턴스의 프로토탕비 체인 상에 존재하지 않기 때문이다
```js
const me = new Person('Hong');
me.sayHi(); // TypeError: me.sayHi is not a function
```

### 정적 메서드와 프로토타입 메서드 차이
```js
class Square {
  // 정적 메서드
  static area(width, height) {
    return width * height;
  }
}
console.log(Square.area(10, 10)); // 100
```
```js
class Square {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  // 프로토타입 메서드
  area() {
    return this.width * this.height;
  }
}
const square = new Square(10, 10);
console.log(square.area()); // 100
// square 객체로 프로토타입 메서드 area를 호출했기 때문에 area 내부의 this는 square 객체를 가리킨다
```
메서드 내부의 this는 메서드를 소유한 객체가 아니라 메서드를 호출한 객체, 이름 앞의 마침표 연산자 앞에 기술한 객체에 바인딩된다
프로토타입 메서드는 인스턴스로 호출해야 하므로 프로토타입 메서드 내부의 this는 프로토타입 메서드를 호출한 인스턴스를 가리킨다
정적 메서드는 클래스로 호출해야 하므로 정적 메서드 내부의 this는 인스턴스가 아닌 클래스를 가리킨다

메서드 내부에서 this를 사용한다면 프로토타입 메서드, this를 사용하지 않는다면 정적 메서드로 호출하는 것이 좋다


## 인스턴스 프로퍼티

