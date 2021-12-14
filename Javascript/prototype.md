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
이처럼 **속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조**를 객체라하며
객체지향 프로그래밍은 독립적인 객체를 집합으로 프로그램을 표현하려는 프로그래밍 패러다임이다

원이라는 개념의 객체
원에는 반지름이라는 속성이 있다
이때 반지름은 원의 **상태를 나타내는 데이터**이며
원의 지름, 둘레, 넓이를 구하는 것이 **동작**이다
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
이처럼 객체지향 프로그래밍은 객체의 **상태**를 나타내는 데이터와
상태 데이터를 조작할 수 있는 **동작**을 하나의 논리적 단위로 묶어 셍각한다
따라서 객체는 **상태 데이터와 동작을 하나의 논리적 단위로 묶은 복합적인 자료구조**라고 할 수 있다
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
**this.radius** 같은 프로퍼티 값은 일반적으로 인스턴스마다 다르다
같은 상태를 가진다면 프로퍼티가 같을 수 있다
그러나 **this.getArea** 같은 메서드는 모든 인스턴스가 동일한 내용의 메서드를 사용하므로
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
**console.log(circle1.getArea === circle2.getArea);**의 구문이 true이다

이것은 자바스크립트 엔진이 프로토타입 기반의 상속에 의한 것이다
Circle 생성자 함수가 생성한 모든 인스턴스는 상위 객체 역할인
Circle.prototype의 모든 프로퍼티와 메서드를 상속 받기 때문이다
getArea는 Circle의 프로토타입으로 단 하나만 생성되어 할당 되어있다
그럼 Circle 생성자 함수로 생성된 모든 인스턴스는 getArea 메서드를 상속받아 사용할 수 있다

즉, 자신의 생태를 나타내는 데이터는 **개별적으로 소유하고 동일한 기능을 나타내는 동작은 상속을 통해 공유**하는 것이다, 이것이 상속이 가지는 코드의 재사용성의 이점이다