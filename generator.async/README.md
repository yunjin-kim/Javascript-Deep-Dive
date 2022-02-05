# 제너레이터

## 제너레이터란?

ES6에 도입된 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다
제너레이터와 일반 함수의 차이
- **제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다**
  - 일반 함수를 호출하면 제어권이 함수에게 넘어가고 함수 코드는 일괄 실행한다, 함수 호출자는 함수를 호출한 이후 함수 실행을 제어할 수 없다
  - 제너레이터 함수는 함수 실행을 함수 호출자가 제어할 수 있다, 함수 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있다
  - 이는 **함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도할 수 있다는 것**을 의미한다
- **제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다**
  - 일반 함수가 호출하면 매개변수를 통해 함수 외부에서 값을 주입받고 함수 코드를 일괄 실행하여 결과값을 함수 외부로 반환한다, 함수가 실행하는 동안 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없다
  - 제너레이터 함수는 함수 호출자와 양방향으로 함수의 상태를 주고받을 수 있다, **제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수 있다**
- **제너레이터 함수를 호출하면 제너레이터 객체를 반환한다**
  - 일반 함수를 호출하면 함수 코드를 일괄 실행하고 값을 반환한다
  - **제너레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다**


## 제너레이터 함수의 정의

제네레이터 함수는 function* 키워드로 선언한다
그리고 하나 이상의 yield 표현식을 포함한다
```js
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
function* genExpFunc() {
  yield 1;
}

// 제너레이터 메서드
const obj = {
  * genObjMethod() {
    yield 1;
  }
}

// 제너레이터 클래스 메서드
class MyClass {
  * genClsMethod() {
    yield 1;
  }
}
```
제너레이터 함수는 화살표 함수로 정의할 수 없고 new 연산자와 함께 생성자 함수로 호출할 수 없다


## 제너레이터 객체

**제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블럭을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다**
**제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터 이다**

제너레이터 객체는 Symbol.iterator 메서드를 상속받는 이터러블이면서
value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유한 이터레이터이다
```js
// 제너레이터 함수 
function* genFunc() {
  try {
  
  } catch(e) {
    yield 1;
    yield 2;
  }
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다
const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.return("End")); // {value: "End", done: true}
console.log(generator.throw("Error")); // {value: undefined, done: true}
```
- throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티값으로 갖는 이터레이터 리절트 객체를 반환한다


## 제너레이터의 일시 중지와 재개

**yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다**

제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지되고 함수의 제어권이 호출자로 양도된다
이때 제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다
next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 yield 표현식에서 yield된 값(yield키워드 뒤의 값)이 할당되고 
done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 불리언 값이 할당된다

이처럼 next 메서드를 반복 호출하여 yield 표현식까지 실행과 일시 중지를 반복하다가 제너레이터 함수가 끝까지 실행되면 
next 메서드가 반환하는 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값이 할당되고 done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었을을 알리는 true가 할당된다

이터레이터의 next 메서드와 달리 제너레이터 객체의 next 메서드에는 인수를 전달할 수 있다
**제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다**
yield 표현식을 할당받는 변수에 yield 표현식의 평가 결과가 할당되지 않는 것을 주의해야 한다
```js
function* genFunc() {
  // x 변수에는 아직 아무것도 할당되지 않았다. x 변수의 값은 next 메서드가 두번째 호출될 때 결정된다
  const x = yield 1;

  // 두번째 next 메서드를 호출할 때 전달한 인수 10은 첫번째 yield 표현식을 할당받는 x 변수에 할당된다
  // 이때 yield 된 값 x + 10은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다
  const y = yield (x + 10);

  // 세번째 next 메서드를 호출할 때 전달한 인수 20은 두번째 yield 표현식을 할당받는 y 변수에 할당된다
  // const y = yield(x + 10); 은 세번째 next 메서드를 호출했을 때 완료된다
  // 이때 제너레이터 함수의 반환값 x + y 는 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다
  // 일반적으로 제너레이터의 반환값은 의미가 없다
  // 따라서 제너레이터에서는 값을 반환할 필요가 없고 return은 종료의 의미로만 사용해야 한다
  return x + y;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다
// 이터러블이면 동시에 이터레이터인 재네레이터 객체는 next 메서드를 갖는다
const generator = genFunc(0);

// 처음 호출하는 next 메서드에는 인수를 전달하지 않는다
// 만약 처음 호출하는 next 메서드에 인수를 전달하면 무시된다
let res = generator.next();
console.log(res); // {value: 1, done: false}

// next 메서드에 인수를 전달한 10은 genFunc 함수의 x 변수에 할당된다
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 두번째 yield 된 값 20이 할당된다
res = generator.next(10);
console.log(res); // {value: 20, done: false}

// next 메서드에 인수로 전달한 20은 genFunc 함수의 y 변수에 할당된다
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값 30이 할당된다
res = generator(20);
console.log(res); // {value: 30, done: true}
```


## 제너레이터 활용

- 피보나치 수열
```js
// 무한 이터러블을 생성하는 함수
const infiniteFibonacci = (function() {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() { return this; },
    next() {
      [pre, cur] = [cur, pre + cur];
      // 무한 이터러블이므로 done 프로퍼티를 생략한다
      return { value: cur };
    }
  }
}());

for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1, 2 3 5 8 ... 2584 4181 6765
}

// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while(true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
}());

for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1, 2 3 5 8 ... 2584 4181 6765
}
```


## async/await

await 키워드는 반드시 async 함수 내부에서 사용해야 한다
async 함수는 async 키워드를 사영해 정의하며 언제나 프로미스를 반횐한다
async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환한다

await 키워드는 프로미스가 settled 상태(비동기 처리가 수행된 상태)가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다
await 키워드는 반드시 프로미스 앞에서 사용해야 한다
```js
const getUsername = asnyc (id) => {
  const res = await fetch(`https://example.com/users/${id}`); // 1
  const { name } = await res.json(); // 2
}
getUsername('qwer1234');
```
fetch 함수가 수행한 HTTP 요청에 대한 서버의 응답이 도착해서 fetch 함수가 반환한 프로미스가 settled 상태가 될때까지 1 은 대기하게 된다
이후 프로미스가 settled 상태가 되면 프로미스가 resolve한 처리 결과가 res 변수에 할당된다