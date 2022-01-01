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
isIterable([]);
isIterable('');
```