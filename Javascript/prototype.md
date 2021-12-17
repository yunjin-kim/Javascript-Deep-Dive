## 프로토타입

### 객체지향 프그래밍

객체지향 프로그래밍은 실세계의 실체(사물이나 개념)를 인식하는 철학적 사고를 프로그래밍에 접목하려는
시도에서 시작한다. 실체는 특징이나 성질을 나타내는 속성을 가지고 있고 이를 통해 실체를
인식하거나 구별할 수 있다

이러한 방식을 프로그래밍에 접목시키면 사람에게는 다양한 속성이 있지만 우리가 구현하려는
프로그램에서는 사람의 '이름', '주소'라는 속성에만 관심이 있다고 가정하면
다양한 속성 중에서 프로그램에 필요한 속성만 간추려 표현하는 것을 추상화라 한다
```js
// 이름과 주소 속성을 갖는 객체
const person = {
  name: 'Hong',
  address: 'Seoul'
};
console.log(person); // {name: "Hong", address: "Seoul"}
```
이떄 프로그래머는 이름과 주소 속성으로 표현된 객체인 person을 다른 객체와 구별해서 인식할 수 있다
이처럼 `속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조`를 객체라하며
객체지향 프로그래밍은 독립적인 객체를 집합으로 프로그램을 표현하려는 프로그래밍 패러다임이다

원이라는 개념의 객체
원에는 반지름이라는 속성이 있다
이때 반지름은 원의 `상태를 나타내는 데이터`이며
원의 지름, 둘레, 넓이를 구하는 것이 `동작`이다
```js
const circle = {
  radius: 5, // 반지름
  // 원의 지름
  getDiameter() {
    return 2 * this.radius;
  },

  // 원의 둘레
  getPerimeter() {
    return 2 * Math.PI * this.radius;
  },

  // 원의 넓이
  getArea() {
    return Math.PI * this.radius ** 2;
  }
};

console.log(circle);
// {radius: 5, getDiameter: f, getPermeter: f, getArea: f}

console.log(circle.getDiameter()); // 10
console.log(circle.getPerimeter()); // 31.41592...
consle.log(circle.getArea()); // 78.53816...
```
이처럼 객체지향 프로그래밍은 객체의 `상태`를 나타내는 데이터와
상태 데이터를 조작할 수 있는 `동작`을 하나의 논리적 단위로 묶어 셍각한다
따라서 객체는 `상태 데이터와 동작을 하나의 논리적 단위로 묶은 복합적인 자료구조`라고 할 수 있다
이때 객체의 상태 데이터를 프로퍼티, 동작을 메서드라고 부른다
이런 객체의 추가적인 기능은
- 자신만의 기능을 수행과 동시에 다른 객체와 관계를 가진다
- 다른 객체와 통신(메시지나 데이터를 송수신 및 처리)할 수 있다
- 다른 객체의 상태 데이터와 동작을 상속받을 수 있다


## 상속과 프로토타입

상속은 어떤 객체가 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다

상속을 통해 불필요한 중복을 제거하고 기존 코드를 재사용함에 있어 중요하다
```js
// 생성자 함수
function Circle(radius) {
  // 프로퍼티
  this.radius = radius;
  // 메서드
  this.getArea = function () {
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // false
console.log(circle1.getArea()); // 3.14...
console.log(circle2.getArea()); // 12.56...
```
생성자 함수는 동일한 프로퍼티 구조를 갖는 객체를 여러개 생성할 때 매우 유용하다
그러나 큰 문제가 있다
`this.radius` 같은 프로퍼티 값은 일반적으로 인스턴스마다 다르다
같은 상태를 가진다면 프로퍼티가 같을 수 있다
그러나 `this.getArea` 같은 메서드는 모든 인스턴스가 동일한 내용의 메서드를 사용하므로
단 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다
그런데 Circle 생성자 함수는 인스턴스를 생성할 때마다 
getArea 메서드를 중복 생성하고 모든 인스턴스를 중복 소유한다

이처럼 동일한 생성자 함수에 의해 생성된 모든 인스턴스가 
동일한 메서드를 중복 소유하는 것은 메모리를 불필요하게 낭비한다
또한 인스턴스를 생성할 때마다 메서드를 생성하므로 10개의 인스턴스를 생성하면 동일한 메서드도 10개 생성된다

이러한 문제를 상속을 통해 불필요한 중복을 제거할 수 있다
자바스크립트는 프로토타입을 기반으로 상속을 구현한다
```js
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// 프로토타입
Circle.prototype.getArea = function() {
  return Math.PI * this.radius ** 2;
}

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.14...
console.log(circle2.getArea()); // 12.56...
```
`console.log(circle1.getArea === circle2.getArea);`의 구문이 true이다

이것은 자바스크립트 엔진이 프로토타입 기반의 상속에 의한 것이다
Circle 생성자 함수가 생성한 모든 인스턴스는 상위 객체 역할인
Circle.prototype의 모든 프로퍼티와 메서드를 상속 받기 때문이다
getArea는 Circle의 프로토타입으로 단 하나만 생성되어 할당 되어있다
그럼 Circle 생성자 함수로 생성된 모든 인스턴스는 getArea 메서드를 상속받아 사용할 수 있다

즉, 자신의 생태를 나타내는 데이터는 `개별적으로 소유하고 동일한 기능을 나타내는 동작은 상속을 통해 공유`하는 것이다, 이것이 상속이 가지는 코드의 재사용성의 이점이다


### 프로토타입 객체

자바스크립트의 모든 객체는 자신의 부모 역할을 하는 객체와 연결되어 있다
이는 객체 지향 프로그래밍의 상속처럼 부모 객체의 기능을 상속받아 사용할 수 있게 한다
이런 부모 객체를 프로토타입 객체, 줄여서 프로토타입이라고 한다

모든 객첸는 `[[Prototype]]`이라는 내부 슬롯을 가지며 해당 슬롯에 저장되는 프로토타입은
객체 생성방식에 의해 결정된다

`[[Prototype]]` 내부슬롯에는 직접 접근할 수 없지만 `__proto__` 접근자 프로퍼티로 내부 슬롯이 가르키는 프로토타입에 간접적으로 접근할 수 있다

- 프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있다
- 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다

결국 모든 객체는 하나의 프로토타입을 갖게 된다
따라서 모든 프로토타입은 생성자 함수와 연결되어 있다고 할 수 있다

### `__poroto__` 접근자 프로퍼티

`모든 객체는 __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입인 [[Prototype]]내부 슬롯에 간접적으로 접근할 수 있다` 

#### `__proto__` 는 접근자 프로퍼티이다

- Object.prototype의 접근자 프로퍼티 `__proto__`는 접근자 함수를 통해 `[[Prototype]] 내부 슬롯 값인 프로토타입을 취득(GET)하거나 할당(SET)한다`
- `__pototype__` 접근자 프로퍼티를 사용하는 프로세스
```js
const obj = {};
const parent = { x: 1 };
// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입 취득
obj.__proto__;
// setter 함수인 set __proto__ 가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

#### `__proto__` 접근자 프로퍼티는 상속을 통해 사용된다

`__proto__` 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 
Object.prototype의 프로퍼티다
모든 객체는 상속을 통해 Object.prototype.`__proto__` 접근자를 사용할 수 있다
```js
const person = { name: 'Hong' };

// person 객체는 __proto__ 프로퍼티를 소유하지 않는다
console.log(person.hasOnPrototype('__proto__')); // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.protoype의 접근자 프로퍼티다
console.log(Object.getOwnPrototypeDescriptor(Object.prototype, '__proto__'));
// {get: f, set: f, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다
console.log({}.__proto__ === Object.prototype); // true
```

#### `__proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유

상호참조로 포로토타입 체인이 생성되는 것을 방지하기 위해서이다
- 상호 참조(Cross-reference), 순환 참조(Circle-reference)라고도 하며 서로가 서로를 참조하는 관계를 말한다
```js
const person = {};
const child = {};

person1.__proto__ = person2;
person2.__proto__ = person1; // TypeError: Cyclic __proto__ value
```
따라서 무조건 프로토타입을 교체할 수 없게, 양방향이 아닌, 단방향으로 연결되게끔
`__proto__` 접근자 프로퍼티를 통해 접근 및 교체가 가능하도록 제약을 걸어놓은 것이다


#### `__proto__` 접근자 프로퍼티를 코드 내에서 직접 사용하는 것을 권장하지 않는다
- proto 접근자 프로퍼티는 ES5까지 비표준이었다, ES6에 와서 표준으로 채택한 것뿐이다
- 권장되지 않는 이유는 모든 객체가 `__proto__` 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문이다
```js
// obj는 프로토타입 체인의 종점이다. 띠리서 Object.__proto를 상속받을 수 없다
const obj = Object.create(null);
console.log(obj.__proto__); // undefined

// 따라서 __proto__ 보단 Object.getPrototypeOf 메서드를 사용하자
console.log(Object.getPrototypeOf(obj)); // null
```
- 프로토타입 참조 취득시 : `Object.getPrototypeOf()`
- 프로토타입 교체 시 : `Object.setPrototypeOf()`


### 함수 객체의 prototype 프로퍼티

함수 객체만이 소유하는 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다

이것이 일반 객체와 함수 객체가 다른 프로토타입을 갖는 이유이다
그러나 생성자 함수로 호출할 수 없는 `non-constructor 함수`는 프로토타입 프로퍼티가 존재하지도, 생성하지도 않습니다
```js
// 함수 객체(construcotor) - 함수 선언문
console.log((function(){}).hasOwnProperty('prototype')); // true

// 함수 객체(constructor) - 함수 표현식
const func = function(){};
console.log(func.hasOwnProperty('prototype')); // true

// 일반 객체
console.log(({}).hasOwnProperty('prototype')); // false

// 화살표 함수(non-contructor)
console.log((() => {}).hasOwnProperty('prototype'), (() =>  {}).prototype); // false, undefined

// 메서드 축약 표현(non-constructor)
const obj = {foo(){}};
console.log(obj.foo.hasOwnProperty('prototype'), obj.foo.prototype); // false, undefined
```
모든 객체가 가진 proto 접근자 프로퍼티와 함수 객체가 가진 prototype 프로퍼티는
동일한 프로토타입을 가리킨다
하지만 이들 프로퍼티가 사용하는 주체가 다르다

|구분|소유|값|사용 주체|사용 목적|
|----|---|---|--------|---------|
|`__proto__`<br>접근자 프로퍼티|모든 객체|프로토타입 참조|모든 객체|객체자기자신 프로토타입<br> 접근|
|`prototype`<br>프로퍼티|constructor|프로토타입 참조|생성자 함수|생성함수가 자신이 생성할<br>인스턴스에 프로토타입 할당|


생성자 함수로 객체를 생성한 후 proto 접근자 프로퍼티와 prototype 프로퍼티로 프로토타입 객체에 접근하기
```js
// 생성자 함수
function Person(name) {
  this.name = nmae;
}
const me = new Person('Hong');
// 결국 Person.prototype과 me.__proto__는 결국 동일한 프로토타입을 가리킨다
console.log(Person.prototype === me.__poroto__); // true
```

### 프로토타입의 constructor 프로퍼티와 생성자 함수

모든 프로토타입은 constructor 프로퍼티를 갖는다

constructor 프로퍼티는 protoype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다
이 연결은 생성자 함수가 생성될 때, 함수 객체가 생성될 때 아루어진다
```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Hong');

console.log(me.constructor === Person); // true
```
- Person 생성자 함수로 me 라는 인스턴스(객체)를 생성한다
- me 객체는 constructor가 없지만 Person.prototype에는 constructor 프로퍼티가 있다
- 따라서 me 객체는 Person.prototype의 constructor 프로퍼티를 상속받아 사용한다


### 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결된다
이 때 constructor 프로퍼티가 가리키는 생성자 함수는 인스턴스를 생성한 생성자 함수다
```js
// obj 객체를 생성한 생성자 함수는 Object다
const obj = new Object();
console.log(obj.constructor === Object); // true

// add 함수 객체를 생성한 생성자 함수는 Function이다
const add = new Function('a', 'b', 'return a + b');
console.log(add.constructor === Function); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}
// me 객체를 생성한 생성자 함수는 Person이다
const me = new Person('Hong');
console.log(me.constructor === Person); // true
```
리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 new 연산자와 함게 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있다
```js
const num = 3;
console.log(num.constructor === Number);

const str = 'str';
console.log(str.constructor === String);

const bool = true;
console.log(bool.constructor === Boolean);

const obj = {};
console.log(obj.constructor === Object);

const add = function(a, b){ return a + b };
console.log(add.constructor === Function);

const arr = [1, 2, 3];
console.log(arr.constructor === Array);

const regexp = /is/gi;
console.log(regexp.constructor === RegExp);
```
리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재한다
하지만 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 순 없다
```js
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체리터럴로 생성헀다
const obj = {};
// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다
console.log(obj.constructor === Object); // true
```
위 예제의 obj 객체는 Object 생성자 함수로 생성한 객체가 아니라
객체 리터럴애ㅔ 의해 생성된 객체다
하지만 obj 객체는 Object 생성자 함수와 constructor 프로퍼티에 연결되어 있다
그렇다면 객체 리터럴에 의해 생성된 객체는 Object 생성자로 생성되는 것인가?

Object 생성자 함수에 인수가 전달하지 않거나 undefined 또는 null을 인수로 전달하면서
호출하면 내부적으로 추상연산 **OrdinaryObjectCreate**를 호출하여
Object.prototype을 프로토타입으로 갖는 빈 객체를 생성한다

```js
// Object 생성자 함수에 의해 객체 생성
// 인수가 전달되지 않았을 때 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성한다
let obj = new Object();
console.log(obj); // {}

// new.target이 undefined나 Object가 아닌 경우
// 인스턴스 -> Foo.prototype -> Object.prototype 순으로 프로토타입 체인이 생성된다
class Foo extends Object {}
new Foo(); // Foo {}

// 인수가 전달될 경우에는ㄴ 인수를 객체로 변환한다
// Number 객체 생성
obj = new Object(123);
console.log(obj); //Number {123}

// String 객체 생성
obj = new Object('123');
console.log(obj); // String {123}
```

이처럼 **생성자 함수 호출**과 **객체 리터럴 평가**는 추상 연산 OrdinaryObjectCreate를
호출하여 빈 객체를 생성하는 점에서 동일하는 new.target 확인이나 프로퍼티를 추가하는 처리등
세부 내용은 다르다
따라서 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다

리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다
따라서 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는다
프로토타입은 생성자 함수와 더불어 생성되며 prototype, constructor 프로퍼티에 의해
연결되어 있기 때문이다
**프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다**

결론은 리터럴에 의해 생성된 객체는 생성자 함수에 의해 생성된 객체가 이니고
생성과정, 스코프, 클로저 등 차이가 있지만
생성된 객체로서 동일한 특성을 가지므로 본질적인 차이는 없다


### 프로토타입 생성 시점