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


## 함수 호출 방식과 this 바인딩

this 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는에
따라 동적으로 결정된다

함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가
생성되는 시점에 상위 스콮프를 결정한다
하지만 this 바인딩은 함수 호출 시점에 결정된다

### 일반 함수 호출
기본적으로 this에는 전역 객체가 바인딩된다

전역 함수와 내부 함수 상관없이 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩 된다 
메서드 내에서 정의한 내부 함수나 콜백 함수도 동일하다
```js
// 일반 함수에 정의한 내부 함수
function foo() {
  console.log(this); // window
  function bar() {
    console.log(this); // window
  }
  "use strict"
  function bar2() {
    console.log(this); // undefined
  }
  bar();
  bar2();
}
foo();

// var 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티다
var value = 1;

// 매서드에서 정의한 내부 함수
const obj = {
  value: 100,
  foo() {
    console.log(this); // {value: 100, foo: f}
    console.log(this.value); // 100
  }

  function bar() {
    console.log(this); // window
    console.log(this.value); // 1
  }
  bar();
};
obj.foo();

// 메서드 내의 콜백 함수
const obj = {
  value: 100,
  foo() {
    console.log(this); // {value: 100, foo: f}
    setTimeout(function () {
      console.log(this); // window
      console.log(this.value) // 1
    }, 100)
  }
}
obj.foo();
```
일반 함수로 호출된 모든 함수(내부함수, 콜백함수) 내부의 this에는 전역 객체가 바인딩된다

메서드의 내부 함수나 콜백 함수의 this 바인딩을 메서드의 내부와 일치시키기 위해서는
this 식별자를 변수에 할당하거나 apply/call/bind 메서드, 화살표 함수를 사용하자
```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    // this 바인딩(obj)을 변수 that에 할당한다
    const that = this;

    setTimeout(function () {
      console.log(that.value); // 100
    }, 100)
  }
};
obj.foo();
```
```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 콜백함수에 명시적으로 this를 바인딩한다
    setTimeout(function () {
      console.log(this.value); // 100
    }.bind(this), 100);
  }
};
obj.foo();
```
```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다
    setTimeout(() => console.log(this.value), 100); // 100
  }
};
obj.foo();
```

## 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체,
즉 메서드를 호출할 때 메서드 이름 앞의 마침표 연산자 앞에 기술한 객체가 바인딩 된다
메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩 된다는 것이다
```js
const person = {
  name: 'Hong',
  getName() {
    // 메서드 내부의 this는 메서드가 호출한 객체에 바인딩된다
    return this.name;
  }
};
// 메서드 getName을 호출한 객체는 person이다
console.log(person.getName()); // Hong

const myName = {
  name: 'Kim'
}
// getName 메서드를 myName 객체의 메서드로 할당
myName.getName = person.getName;

//getName 메서드를 호출한 객체는 myName이다
console.log(myName.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

//getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
```
일반 함수로 호출된 getName 함수 내부의 this.name은브라우저 환경에서 window.name과 같다
브라우저의 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이다
따라서 기본값은 ''이 된다, Node.js 에서는 undefined이다

이는 프로토타입 내부에서 사용된 this 바인딩도 동일하다
```js
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person('Hong');
console.log(me.getName()); // Hong    

Person.prototype.name = 'Kim';
console.log(Person.prototype.getName()); // Kim
```

## 생성자 함수 호출