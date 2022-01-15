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