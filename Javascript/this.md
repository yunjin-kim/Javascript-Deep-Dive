## this 키워드

객체는 생태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적 단위로 묶은 복합적 요소이다.

동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다.
이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어햐 한다.

객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

객체 리터럴 방식으로 선언한 경우
```js
const circle = {
  // 프로퍼티: 객체 고유의 상태 데이터
  radius: 5,
  // 메서드: 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    // 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
    // 자신이 속한 객체인 circle을 참조할 수 있어야 한다
    return 2 * circle.radius;
  }
};
console.log(circle.getDiameter()); // 10
```
- getDiameter를 호출하면 circle을 재귀적으로 호출한다.
  이 참조 표횬식이 평가되는 시점은 getDiameter 메서드가 호출되어 함수 몸체가 실행되는 시점이다.
- 객체 리터럴은 변수에 할당되기 전에 평가된다. 따라서 getDiameter가 호출되는 시점에는
  객체가 생성된 시점이므로 메서드 내부에서 해당 식별자(circle)를 참조할 수 있다.
  하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 것은 바람직하지 않다.

생성자 함수 방식으로 선언한 경우
```js
function Circle(radius) {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다
  ??.radius = radius;
}

Circle.prototype.getDiameter = function() {
  // 마찬가지이다
  return 2 * ??.radius;
}

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다
const circle = new Circle(5);
```
- 생성자 함수 내부에선는 자신이 생성한 인스턴스를 참조할 수 있어야 한다. 그러나 생성자
  함수가 호출되지 않은 이상 인스턴스는 생성되지 않는다
- 결국 생성자 함술르 정의하는 시점에는 이 생성자 함수가 어떤 인스턴스를 생성하는지 식별자는
  무엇인지 알 수 없다

결국 자신이 속한 객체나 생성한 인스턴스를 가리킬 식별자가 필요한데 이게 this 이다
자기 참조 변수로서 자신이 속한 객체 또는 자신이 생성할 인스턴스나 프로퍼티나 메서드를 참조할 수 있다
- this는 자바스크립트 엔진에 의해 암묵적으로 생성된다
- 그래서 코드 어디서든 참조할 수 있다
- 함수를 호출하면 arguments 객체와 this가 암묵적으로 전달된다
- 함수 내부에서 arguments 객체를 지역 변수처럼 사용할 수 있는 것처럼 this도 지역변수처럼 사용할 수 있다
- this가 가리키는값, this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다

객체 리터럴 방식으로 선언한 경우
```js
const circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체를 가리킨다
    return 2 * this.radius;
  }
};
console.log(circle.getDiameter()); // 10
```

생성자 함수 방식으로 선언한 경우
```js
function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다
  this.radius = radius;
}

Circle.prototype.getDiameter = function() {
  // 마찬가지이다
  return 2 * this.radius;
}

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

this 바인딩이란 식별자와 값을 연결하는 과정으로 this(키워드, 식별자로 분류)와 this가
가리킬 객체를 연결하는 것이다
자바스크립트의 this는 함수가 호출되는 방식에 따라 this가 비인딩 될 값,
즉 this 바인딩이 동적으로 결정된다

this는 자기 참조 변수이다
일반 함수 내부에서는 사용할 필요가 없기 때문에 
strict mode에서는 undefined가 할당되고
기본 자바스크립트 엔진은 전역 객체에 바인딩된 상태이므로 window를 출력한다