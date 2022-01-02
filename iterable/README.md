## 이터러블


### 이터레이션 프로토콜

ES6에 도입된 이터레이션 프로토콜(iteration protocol)은 순회 가능한(iterable) 데이터 컬렉션(자료 구조)을 만들기 위해 ECMA Script 사양에 정의하여 미리 약속한 규칙이다

이터레이션 프로토콜에는 이터러블 프로토콜과 이터레이터 프로토콜이 있다


- 이터러블 프로토콜(iterable protocol)

Well-known Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속 받은 Symbol.iterator 메서드를 호출하면
이터레이터 프로토콜을 준수한 이터레이터를 반환한다
이런 규약을 **이터러블 프로토콜**이라고 하며 **이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 이터러블은 for ...of 문으로 순회할 수 있으며 스프레드 문법과 배열 드스터럭처링 할당의 대상으로 사용할 수 있다**

- 이터레이터 프로토콜(iterator protocol)

이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 **이터레이터**를 반환한다
이터레이터는 next 메서드를 호출하면 이터러블 순회하며 value와 done 프로퍼티를 갖는 **이터레이터 리절트 객체**를 반환한다
이러한 규약을 이터레이터 프로토콜이라 하며, **이터레이터 프로토콜을 준수한 객체를 이터레이터**라 한다.
이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할을 한다

이터레이션 프로토콜
```js
// iterable  순회 가능한 자료 구조           iterator   이터러블의 요소를 탐색하기 위한 포인터
                                        {
                                          next() {    // 이터레이터 리절트 객체
[Symbol.iterator]() { ... }                 return { value: any, done: boolean };
                                          }
                                        }
```


### 이터러블

이터러블 프로토콜을 준수한 객체를 이터러블이라 한다
즉 이터러블은 **Symbol.iterator**를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말한다
```js
const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function';

// 배열, 문자열, Map, Set 등은 이터러블이다
isIterable([]);         // true
isIterable('');         // true
isIterable(new Map());  // true
isIterable(new Set());  // true
isIterable({});         // false
```

이터러블은 **for ... of 문**으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다
```js
const array = [1, 2, 3];
console.log(Symbol.iterator on array); // true

// 이터러블인 배열은 for ... of 문으로 순회가 가능하다
for (const item of array) {
  console.log(item);
}

// 이터러블인 배열은 스프레드 문법의 대상으로 사용할 수 있다
console.log([...array]); // [1, 2, 3]

// 이터러블인 배열은 배열 디스트럭처링 할당의 대상으로 사용할 수 있다
const [a, ...rest] = array;
console.log(a, rest); // 1, [2, 3]
```

Symbol.iterator 메서드를 직접 구현하지 않거나 상속 받지 않은 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다
따라서 일반 객체는 **for ... of 문**으로 순회할 수 없으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 없다
```js
const obj = { a: 1, b: 2 };

// 일반 객체는 Symbol.iterator 메서드를 구현하거나 상속 받지 않는다
// 따라서 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다
console.log(Symbol.iterator in obj); // false

// 이터러블이 아닌 일반 객체는 for ... of 문으로 순회할 수 없다
for (const item of obj) {
  console.log(item);
}

// 이터러블이 아닌 일반 객체는 배열 디스트럽처링 할당의 대상으로 사용할 수 없다
const [a, b] = obj; // TypeError
```

하지만 일반 객체도 이터러블 프로토콜을 준수하도록 구현하면 이터러블이 된다


## 이터레이터

이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다
**이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다**
```js
// 배열은 이터러블 프로토콜을 준수한 이터러블이다
const array = [1, 2, 3];

// Symbol.iterator 메서드는 어터레이터를 반환한다
const iterator = array[Symbol.iterator]();

// Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다
console.log('next' in iterator); // true
```

이터레이텅의 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다
next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 **이터레이터 리절트 객체**를 반환한다

```js
const array = [1, 2, 3];
const iterator = array[Symbol.iterator]();

// next 메서드를 호출하면 이터러블을 순회하며 수뇌 결과를 나타내는 이터레이터 리절트 객체를 반환한다
// 이터레이터 리절트 객체는 value와 done 프로퍼티를 갖는 객체다
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

이터레이터의 next 메서드가 반환하는 이터레이터 리절트 객체의 value 프로퍼티는 현재 순회 중인 이터러블의 값을 나타내며
done 프로퍼티는 이터러블 순회 완료 여부를 나타낸다


## 빌트인 이터러블

자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블을 제공한다

|빌트인 이터러블|Symbol.iterator 메서드|
|------|----------|
|Array|Array.prototype[Symbol.iterator]|
|String|String.prototype[Symbol.iterator]|
|Map|Map.prototype[Symbol.iterator]|
|Set|Set.prototype[Symbol.iterator]|
|TypedArray|TypedArray.prototype[Symbol.iterator]|
|arguments|arguments.prototype[Symbol.iterator]|
|DOM 컬렉션|NodeList.prototype[Symbol.iterator], HTMLCollection.prototype[Symbol.iterator]|


## for ... of 문

for ... of 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다
```js
for (변수선언문 of 이터러블) { ... }
```

for ... of 문은 for ... in 문의 형식과 매우 유사하다
```js
for (변수선언문 in 객체) { ... }
```

for ... in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트**[[Enumerable]]**의 값이
true인 프로퍼티를 순회하며 열거한다. 이 때 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다

for ... of 문은 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 
next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for ... of 문의 변수에 할당한다
그리고 이터레이터 리절트 객체의 done 프로퍼티 값이 false이면 이터러블의 수뇌를 계속하고 true이면 순회를 중단한다

- for ... of 문의 내부 동작을 for 문으로 표현
```js
// 이터러블
const iterable = [1, 2, 3];

// 이터러블의 Symbol.iterator 메서드를 호출하여 이터레이터를 생성한다
const iterator = iterable[Symbol.iterator]();

for (;;) {
  // 이터레이터의 next 메서드를 호출하여 이터러블을 순회한다
  // 이때 next 메서드는 이터레이터 리절트 객체를 반환한다
  const res = iterator.next();

  // next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 true이면 이터러블의 순회를 종료한다
  if (res.done) break;

  // 이터레이터 리절트 객체의 value 프로퍼티 값을 item 변수에 할당한다
  const item = res.value;
  console.log(item); // 1 2 3
}

```


## 이터러블과 유사 배열 객체