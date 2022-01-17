# 프로퍼티 어트리뷰트

## 내부 슬롯과 내부 메서드

프로퍼티 어트리뷰트를 이해하기 전에 내부 슬롯(internal slot)과 내부 메서드(internal method)의 개념을 알아야 한다

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서
사용하는 의사 프로퍼티(pseudo property)와 의사 메서드(pseudo method)다
ECMAScript 사양에 등장하는 이중 대괄호 [[...]]로 감싼 이름들이 내부 슬롯과 내부 메서드다

내부 슬롯과 내부 메서드는 개발자가 직접 접근할 수 없다
단, 일부 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있다

예를 들어 모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다
이 내부 슬롯은 __proto__를 통해 간접적으로 접근할 수 있다


## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

**자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본적으로 자동 정의한다**

프로퍼티의 상태란 프로퍼티의 값, 값의 갱신 가능 여부, 열거 가능 여부, 재정의 가능여부를 말한다
```js
const person = {
  name: 'Hong'
}

//프로퍼티 동적 생성
person.age = 20;

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다
console.log(Object.getOwnPropertyDescriptors(person));
/*
  {
    name: {value: "Hong", writable: true, enumerable: true, configurable: true},
    age: {value: 20, writable: true, enumerable: true, configurable: true}
  }
*/
```

## 데이터 프로퍼티

- 데이터 프로퍼티 (data property)
  - 키와 값으로 구성된 일반적인 프로퍼티다

|프로퍼티 어트리뷰트|프로퍼티 디스크립터 객체의 프로퍼티|설명|
|---------------|--------------|------------------|
|[[Value]]|value| 프로퍼티 키를 통해 값에 접근하면 반환되는 값이다, 프로퍼티 키를 통해 프로퍼티에 값을 변경하면 [[Value]]에 값을 재할당한다. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장한다|
|[[Wiritable]]|writable| 프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값을 갖는다, [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다|
|[[Enumerable]]|enumerable| 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다, [[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for ... in문이나 Object.keys 메서드로 열거할 수 없다|
|[[Configurable]]|configurable| 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다, [[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다, 단 [[Wiritable]]이 true인 경우 [[Value]]의 변경과  [[Wiritable]]을 false로 변경하는 것이 가능하다|


## 접근자 프로퍼티

- 접근자 프로퍼티 (accessor property)
  - 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티를 읽거나 저장할 때 호출되는 전근자 프로퍼티로 구성된 프로퍼티다

|프로퍼티 어트리뷰트|프로퍼티 디스크립터 객체의 프로퍼티|설명|
|---------------|--------------|------------------|
|[[Get]]| get | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수, 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값, getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환된다|
|[[Set]]| set | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수, 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값, setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다|
|[[Enumerable]]| enumerable | 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다, [[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for ... in문이나 Object.keys 메서드로 열거할 수 없다|
|[[Configurable]]|configurable| 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다, [[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다, 단 [[Wiritable]]이 true인 경우 [[Value]]의 변경과  [[Wiritable]]을 false로 변경하는 것이 가능하다|

```js
const person = {
  firstName: 'siyoung',
  lastName: 'Hong'

  // getter 함수
  get myName() {
    return `${this.firstName} ${this.lastName}`
  }
  // setter 함수
  set myName(name) {
    [this.firstName, this.lastName] = name.split('');
  }
};

// 접근자 프로퍼티를 통한 프로퍼티 값 저장, setter함수 호출
person.myName = 'giyoung Yun';

// 접근자 프로퍼티를 통한 프로퍼티 값 참조, getter함수 호출
console.log(person.myName);

// firstName은 데이터 프로퍼티
let descriptor = Object.getOwnPropertyDescriptors(person, 'firstName');
console.log(descriptor);
// {value: "giyoung", writable: true, enumerable: true, configurable: true}

// myName은 접근자 프로퍼티다
descriptor =  Object.getOwnPropertyDescriptors(person, 'myName');
console.log(descriptor);
// {get: f, set: f, enumerable: true, configurable: true}
```

접근자 프로퍼티는 자체적으로 값(프로퍼티 어트리뷰트[[Value]])을 가지지 않으며 다만 데이터 프로퍼티의 값을 읽거나 저장만 한다


## 프로퍼티 정의

프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다
예를 들어 프로퍼티 값을 갱신 가능하도록 할 것인지, 프로퍼티를 열거 가능하게 할 것인지, 프로퍼티를 재정의 가능하도록 할 것인지 정의할 수 있다
이를 통해 객체의 프로퍼티가 어떻게 동작해야 하는지 명확히 정의할 수 있다

Object.defineProperty 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다

```js
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person , {
  'firstName': {
    value: 'Siyoung',
    writable: true,
    enumerable: true,
    configurable: true
  },
  lastName: {
    value: 'Hong',
    writable: true,
    enumerable: true,
    configurable: true
  },
  // 접근자 프로퍼티 정의
  fullNane: {
    get() {
      return `${this.firstName}${this.lastName}`;
    },
    set(name) {
      [this.firstName, this.lastName] = name.split(' ');
    },
    enumerable: true,
    configurable: true
  }
});

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
// { value: "Hong", writable: true, enumerable: true, configurable: true }
```

## 객체 변경 방지

### 객체 확장 금지
Object.preventExtensions 메서드는 객체의 확장을 금지한다
객체 확장 금지란 프로퍼티 추가 금지를 의미한다
```js
const person = { name: 'Hong' };
// person 객체는 확장이 금지된 객체가 아니다
console.log(Object.isExtensible(person)); // true

// person 객체의 확장을 금지한다
Object.preventExtensions(person);

// person 객체는 확장이 금지된 객체다
console.log(Object.isExtensible(person)); // false

// 프로퍼티 추가가 금지된다
person.age = 20; // 무시
console.log(person); // { name: "Hong" }

// 프로퍼티 삭제는 가능
delete person.name;
console.log(person); // {}

// 프로퍼티 정의에 의한 프로퍼티 추가도 금지
Object.defineProperty(person, 'age', { value: 20 }); // TypeError
```

### 객체 밀봉

Object.seal 메서드는 객체를 밀봉한다
객체 밀봉이란 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지를 말한다
밀봉된 객체는 읽기와 쓰기만 가능하다
```js
const person = { name: 'Hong' };

// person 객체는 밀봉된 객체가 아니다
console.log(Object.isSealed(person)); // false

// person 객체를 밀봉하여 프로퍼티 추가, 삭제, 재정의를 금지한다
Object.seal(person);

// person 객체는 밀봉된 객체다
console.log(Object.isSealed(person)); // true

// 프로퍼티 추가, 삭제, 재정의는 금지되지만 값 갱신은 가능하다
perosn.name = 'Lee';
console.log(person); // { name: 'Lee' }
```

### 객체 동결

Object.freeze 메서드는 객체를 동결한다
객체 동결이란 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지, 프로퍼티 값 갱신 금지를 의미한다
동결된 객체는 읽기만 가능하다
```js
const name = { name: 'Hong' };

// person 객체는 동결된 객체가 아니다
console.log(Object.isFrozen(person)); // false

// person 객첼르 동결하여 프로퍼티 추가, 삭제, 재정의, 쓰기가 금지된다
Object.freeze(person);

// person 객체는 동결된 객체다
console.log(Object.isFrozen(person)); // true
```

## 불변 객체

위 세 메서드는 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지는 못한다
```js
const person = {
  name: 'Hong',
  address: { city: 'Seoul' }
};

// 얕은 객체 동결
Object.freeze(person);

// 중첩 객체까지는 동결하지 못한다
console.log(Object.isFrozen(person.address)); // false

person.address.city = 'Busan';
console.log(person); // { name: "Hong", address: { city: "Busan" }}
```

객체의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 불변 객체를 구현하려면 객체를 값으로 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야한다
```js
function deepFreeze(target) {
  if (target && typeof target === 'object' && !Object.isFrozen(target)) {
    Object.freeze(target);
    Object.keys(target).forEach((key) => deepFreeze(target[key]));
  }
  return target;
}

const person = {
  name: 'Hong',
  address: { city: 'Seoul' }
};

// 깊은 객체 동결
deepFreeze(person);
console.log(Object.isFrozen(person.address)); // true
```


## 정리
자바스크립트에는 불변한 값을 만드는 방법이 다른 언어에 비해 부족하다
변경되기 쉬운 값인 객체를 변경하기 힘든 값으로 만드는 것을 
- Object.preventExtensions(obj);
- Object.seal(obj);
- Object.freeze(obj);
- deepFreeze 함수
를 활용하면 좀 더 안전한 값을 만들 수 있다