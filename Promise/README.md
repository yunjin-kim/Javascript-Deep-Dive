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


### 프로미스 에러 처리