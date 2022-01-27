# 프로미스

비동기 함수를 호출하면 함수 내부의 비동기로 동작하지 않는 코드가 완료되지 않았다 해도 기다리지 않고 즉시 종료된다
즉, 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료된다
따라서 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다

비동기 함수는 비동기 처리 결과를 외부에 반환할 수 업고 상위 스코프의 변수에 할당할 수도 없다
따라서 비동기 함수의 처리 결과에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다
이때 비동기 함수를 범용적으로 사용하기 위해 비동기 함수에 비동기 처리결과에 대한 후속처리를 수행하는 콜백 함수를 전달하는 것이 일반적이다
필요에 따라 비동기 처리가 성공하면 호출될 콜백 함수와 비동기 처리가 실패하면 호출될 콜백 함수를 전달할 수 있다


### 에러 처리의 한계

비동기 처리를 위한 콜백 패턴의 문제점 중 가장 심각한 것은 에러 처리가 곤란하다는 것이다
```js
try {
  setTimeout(() => { throw new Error('Error') }, 1000);
} catch (e) {
  // 에러를 캐치하지 못한다
  console.log('error', e);
}
```
try 코드 블럭에서 호출한 setTimeout 함수는 1초후 에러가 발생시킨다
하지만 이 에러는 catch 코드 블록에서 캐치되지 않는다
- 비동기 함수인 setTimeout 함수가 호출되면 setTimeout 함수의 실행 컨텍스트가 생성되어 콜 스택에 푸시되어 실행된다
- setTimeout 함수는 비동기 함수이므로 콜백 함수가 호출되는 것을 기다리지 않고 즉시 종료되어 콜 스택에서 제거된다
- 이후 1초가 지나면 콜백 함수는 태스크 큐로 푸시되고 콜 스택이 비어졌을 때 이벤트 루프에 의해 콜 스택으로 푸시되어 실행된다
- setTimeout 함수의 콜백 함수가 실행될 때 setTimeout 함수는 이미 콜 스택에서 제거되었다(이것은 setTimeout 함수의 콜백 함수를 호출된 것이 setTimeout이 아니라는 것을 의미한다)
- setTimeout 함수의 콜백 함수의 호출자가 setTimeout 함수라면 현재 실행 중인 실행 컨텍스트(콜백 함수)의 하위 실행 컨텍스트가 setTimeout 함수여야 한다
- **에러는 호출자 방향으로 전파된다**
- setTimeout 함수의 콜백 함수를 호출한 것은 setTimeout 함수가 아니라
- 따라서 setTimeout 함수의 콜백 함수가 발생시킨 에러는 catch 블록에서 캐치되지 않는다

이를 극복하기 위해 프로미스가 등장했다


## 프로미스 생성
```js
//프로미스 생성
const promise = new Promise((resolve, reject) => {
  // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행한다
  if (/* 비동기처리 성공 */) {
    resolve('result');
  } else { // 실패
    reject('failure reason');
  }
});
```

- 프로미스의 상태 정보

|프로미스 상태 정보|의미|상태 변경 조건|
|------|--------------|------------------|
|pending|비동기 처리가 아직 수행되지 않은 상태|프로미스가 생성된 직후 기본 상태|
|fulfilled|비동기 처리가 수행된 상태(성공)|resolve 함수 호출|
|rejected|비동기 처리가 수행된 상태(실패)|reject 함수 호출|

**프로미스의 상태는 resolve, reject 함수를 호출하는 것으로 결정된다**

fulfilled 또는 rejected 상태를 settled 상태라고 한다
settled 상태는 fulfilled 또는 rejected 상태와 상관없이 pending이 아닌 상태로 비동기 처리가 수행된 상태를 말한다

프로미스는 pending 상태에서 fulfilled 또는 rejected 상태, 즉 settled 상태로 변화할 수 있다
히자만 일단 settled 상태가 되면 더는 다른 상태로 변화할 수 없다

**프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체다**


### 프로마스 후속처리 메서드
프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출된다

#### Promise.prototype.then
then 메서드는 두개의 콜백 함수를 인수로 받는다
- 첫번째 콜백 함수는 프로미스가 fulfilled 상태가 되면 호출된다. 이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받는다
- 두번째 콜백 함수는 프로미스가 rejected 상태가 되면 호출된다. 이때 콜백 함수는 프로미스의 에러를 인수로 전달받는다
```js
new Promise(resolve => resolve('fulfilled'))
  .then(v => console.log(v), e => console.error(e)); // fulfill
```

#### Promise.prototype.catch
catch 메서드는 한 개의 콜백 함수를 인수로 전달받는다
catch 메서드의 콜백 함수는 프로미스가 rejected 상태인 경우만 호출된다
```js
new Promise((_, reject) => reject(new Error('rejected')))
  .catch(e => console.log(e)); // Error: rejected
```

#### Promise.prototype.finally
finally 메서드는 한개의 콜백 함수를 인수로 전달받는다
finally 메서드는 프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용하다
finally 메서드도 언제나 프로미스를 반환한다
```js
new Promise(() => {})
  .finally(() => console.log('finally')); // finally
```


### 프로미스 체이닝
then, catch, finally 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있다
이를 프로미스 체이닝이라 한다

|후속 처리 메서드|콜백 함수의 인수|후속 처리 메서드의 반환값|
|------|--------------|------------------|
|then|promiseGet 함수가 반환한 프로미스가 resolve한 값|콜백 함수가 반환한 프로미스|
|then|첫번째 then 메서드가 반환한 프로미스가 resolve한 값|콜백 함수가 반환한 값을 resolve한 프로미스|
|catch|promiseGet 함수 또는 앞선 후속 처리 메서드가 반환한 프로미스가 rejec한 값|콜백 함수가 반환한 값을 resolve한 프로미스|

프로미스도 콜백 패턴을 사용하지 않는 것은 아니다
콜백 패턴은 가독성이 좋지 않아 asnyc/await 을 사용하는 편이 좋다


### 프로미스의 정적 메서드

#### Promise.resolve / Promise.reject
Promise.resolve와 Promise.reject 메서드는 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용한다

Promise.resolve 메서드는 인수로 전달받은 값을 resolve하는 프로미스를 생성한다
```js
// 배열을 resolve하는 프로미스
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log); // [1, 2, 3]

// 동일하게 동작한다
const resolvedPromise = new Promise(resolve => resolve([1, 2, 3]));
resolvedPromise.then(console.log); // [1, 2, 3]
```

Promise.reject 메서드는 인수로 전달받은 값을 reject하는 프로미스를 생성한다
```js
// 에러객체를 reject하는 프로미스
const rejectdPromise = Promise.reject(new Error('Error'));
rejectdPromise.catch(console.log); // Error: Error

// 동일하게 동작한다
const rejectdPromise = new Promise((_, reject) => reject(new Error('Error')));
rejectdPromise.catch(console.log); // Error: Error
```

#### Promise.all
Promise.all 메서드는 여러개의 비동기 처리를 모두 병렬 처리할떄 사용한다
```js
const requestData1 = () => 
  new Promise(resolve => setTimeout(() => resolve(1), 3000));
const requestData2 = () => 
  new Promise(resolve => setTimeout(() => resolve(2), 2000));
const requestData3 = () => 
  new Promise(resolve => setTimeout(() => resolve(3), 1000));

// 세개의 비동기 처리를 병렬로 처리
Promise.all([requestData1(), requestData2(), requestData3()])
  .then(console.log) // [1, 2, 3] 약 3초 소요
  .catch(console.error);
```
위 예제는 다음과 같이 동작한다
- 첫번째 프로미스는 3초 후에 1을 resolve한다
- 두번째 프로미스는 2초 후에 2을 resolve한다
- 세번째 프로미스는 1초 후에 3을 resolve한다

Promise.all 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다
그리고 전달받은 모든 프로미스가 모두 fulfilled 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스로 반환한다
따라서 Promise.all 메서드가 종료하는데 걸리는 시간은 가장 늦게 fulfilled 상태가 되는 프로미스의 처리 시간보다 조금 더 길다
Promise.all 메서드는 첫번째 프로미스가 resolve한 처리 결과부터 차례대로 배열에 저장해 그 배열을 resolve하는 새로운 프로미스를 반환한다
**치리 순서가 보장된다**
Promise.all 메서드는 인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 
나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 에러를 뿜으며 즉시 종료한다

Promise.all 메서드는 인수로 전달 받은 이터러블의 요소가 프로미스가 아닌 경우 Promise.resolve 메서드를 통해 프로미스로 래핑한다
```js
Promise.all([
  1, // -> Promise.resolve(1)
  2, // -> Promise.resolve(2)
  3, // -> Promise.resolve(3)
])
  .then(console.log) // [1, 2, 3]
  .catch(console.log);
```

#### Promise.race
Promise.race 메서드는 Promise.all 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 받는다
Promise.race 메서드는 Promise.all 메서드처럼 모든 프로미스가 fulfilled 상태가 되는 것으 기다리는 것이 아니라
가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스를 반환한다
```js
Promise.race([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000)), // 3
])
.then(console.log) // 3
.catch(console.log)
```
프로미스가 rejected 상태가 되면 Promise.all과 동일하게 처리된다
Promise.race에 전달된 프로미스가 하나라도 rejected 상태가 되면 에러를 reject하는 새로운 프로미스를 즉시 반환한다


#### Promise.allSettled
Promise.allSettled 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 받는다
그리고 전달받은 프로미스가 모두 settled 상태(비동기 처리가 수행된 상태, 즉 fulfulled 또는 rejected 상태)가 되면 처리 결과를 배열로 반환한다
Promise.allSettled 메서드는 IE를 제외하고 지원한다
```js
Promise.allSettled([
  new Promise(resolve => setTimeout(() => resolve(1), 2000)),
  new Promise((_, reject) => setTimeout(() => reject(new Error('Error!')), 1000))
]).then(console.log)
/*
[
  {status: "fulfilled", value: 1},
  {staus: "rejected", reason: Error: Error! at <anonymous>}
]
*/
```
Promise.allSettled 메서드가 반환한 배열에는 fulfilled 또는 rejected 상태와는 상관없이 Promise.allSettled 메서드가 인수로 전달받은 모든 프로미스들의 처리 결과가 모두 담겨있다
프로미스의 처리 결과를 나타내는 객체는 다음과 같다
- 프로미스가 fulfulled 상태인 경우 비동기 처리 상태를 나타내는 status 프로퍼티와 처리 결과를 나타내는 value 프로퍼티를 갖는다
- 프로미스가 rejected 상태인 경우 비동기 처리 상태를 나타내는 status 프로퍼티와 에러를 나타내는 reason 프로퍼티를 갖는다

## 정리
Promise.resolve/rejct 는 프로미스를 생성하기 위해 값을 래핑한다
Promise.all 은 여러 개의 비동기 처리를 할 수 있는데 제일 오래 걸리는 비동기 처리가 끝나면 비동기 함수가 작성한 순서대로 처리된다, rejected 상태가 하나라도 생기면 순간 즉시 종료하고 에러를 뿜는다
Promise.race 는 여러 개의 비동기 처리를 할 수 있는데 먼저 끝나는 비동기 함수부터 처리된다, rejected 상태가 하나라도 생기면 순간 즉시 종료하고 에러를 뿜는다
Promise.allSettled 는 fulfilled 상태의 비동기는 처리결과를 value 프로퍼티를 갖고 rejected 상태의 비동기 처리는 reason 프로퍼티에서 에러를 나타낸다
상황에 맞게 잘 사용하면 좋을 것 같음!


## 마이크로태스크 큐
