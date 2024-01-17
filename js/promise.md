### Q. callback은 무엇인가요?

callback은 매개변수로 함수 객체를 전달해서 호출 함수 내에서 매개변수 함수를 실행하는 것을 말한다.

콜백은 비동기 처리에서 사용될 수 있다

무언가를 비동기적으로 수행하는 함수는 함수 내 동작이 모두 처리된 후 실행되어야 하는 함수를 인자로 받아서 처리가 완료된 후에 실행되도록 할 수 있다.

### Q. callback의 문제점은 뭔가요?

callback을 중첩 호출하기 위해서는 함수를 중첩해서 호출해야 한다.
이는 코드의 가독성을 매우 떨어뜨리며 관리하기 힘들게 만든다.
이는 콜백 지옥이라는 문제로 이어진다.

### Q. 콜백 지옥문제는 어떻게 해결할 수 있나요?

Promise를 사용하면 콜백 지옥 문제를 해결할 수 있다.

### Q. Promise는 무엇인가요?

Promise는 자바스크립트 비동기 처리를 위한 객체다.
Promise는 내부적으로 state 속성을 갖고 있다.

```js
let promise = new Promise(function (resolve, reject) {
  // executor
  setTimeout(() => resolve('완료'), 1000)
  reject(new Error('…'))
})
```

위는 기본적인 Promise의 구조이다.Promise의 executor는 resolve 또는 reject를 호출한다.

resolve는 성공시 실행되며 프로미스 객체의 상태를 fulfilled로 변경하고 결과값을 프로미스 객체의 result 속성에 저장한다.

reject는 실패시 실행되며 프로미스 객체의 상태를 rejected로 변경하고 에러를 프로미스 객체의 error 속성에 저장한다.

### Q. Promise 어떻게 사용하나요?

Promise는 3개의 소비자가 있다.

#### then

```ts
promise.then(
  function (result) {
    /* 결과(result)를 다룹니다 */
  },
  function (error) {
    /* 에러(error)를 다룹니다 */
  }
)
```

then메서드는 완료된 프로미스의 상태에따라 다른 함수를 실행한다. 이 함수들은 프로미스가 완료될 때만 실행되기 때문에 비동기적인 처리를 할 수 있다.

#### catch

catch는 프로미스 이행 과정에서 발생한 에러를 처리하기 위해 사용된다.
catch는 프로미스가 rejected된 경우 실행된다.
catch는 then(null, f)와 동일하다.

#### finally

finally는 프로미스 이행 결과와 상관없이 실행된다.

### Q. 이것으로 어떻게 콜백 지옥을 해결할 수 있나요?

기존 콜백 지옥을 유발하는 콜백함수가 프로미스 객체를 반환하게 하면 된다.

기존 스스로 함수 내부에서 자신의 실행을 마무리하면 콜백을 실행하는 구조에서 실행이 마무리 되면 프로미스 생성자 내부에서 resolve를 호출하면 된다.

그리고 함수 외부에서는 then을 사용하여 콜백을 등록하면 된다.

만약 비동기 호출을 중첩하여 호출해야 한다면 프로미스 체이닝을 사용하면 된다.

### Q. 프로미스 체이닝이 뭔가요?

```ts
new Promise(function (resolve, reject) {
  setTimeout(() => resolve(1), 1000) // (*)
})
  .then(function (result) {
    // (**)

    alert(result) // 1
    return result * 2
  })
  .then(function (result) {
    // (***)

    alert(result) // 2
    return result * 2
  })
  .then(function (result) {
    alert(result) // 4
    return result * 2
  })
```

then메서드는 다음 then메서드에 반환값을 전달할 수 있다.
이를 통해 비동기 여러번 순차적으로 호출할 수 있다.
이 과정에서 콜백 환경에서 발생했던 콜백 지옥현상을 해결한다.

### Q. 프로미스 체이닝은 어떻게 가능한가요?

then메서드가 프로미스를 반환하기 때문이다. then메서드에서 그대로 값을 반환하면 Promise.resolve(value)를 반환하는 것과 같다.

### Q. 프로미스의 API에 대해 설명해주세요

#### Promise.all

Promise.all은 프로미스 배열을 받고 새로운 프로미스 객체를 반환한다.
배열 안의 프로미스가 모두 resolve되면 반환되는 프로미스가 resolve된다.반환 받은 프로미스의 result는 배열 내부 프로미스의 result를 담은 배열이다.

```ts
Promise.all([
  new Promise((resolve) => setTimeout(() => resolve(1), 3000)), // 1
  new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
  new Promise((resolve) => setTimeout(() => resolve(3), 1000)), // 3
]).then(alert) // 프라미스 전체가 처리되면 1, 2, 3이 반환된다.
```

Promise.all에 전달되는 프라미스 중 하나라도 거부되면, Promise.all이 반환하는 프라미스는 에러와 함께 바로 전체가 거부된다. 에러가 발생하지 않은 다른 프로미스는 무시된다.

#### Promise.allSettled

Promise.allSettled는 Promise.all과 비슷하지만, 하나의 프로미스가 실패해도 전체가 거부되지 않고, 모든 result가 담긴 배열을 반환한다.

```ts
results = [
  {status: 'fulfilled', value: ...응답...},
  {status: 'fulfilled', value: ...응답...},
  {status: 'rejected', reason: ...에러 객체...}
]
```

#### Promise.race

Promise.race 또한 Promise.all과 유사합니다. 다만 가장 먼저 처리된 프로미스의 결과만 반환합니다.

```ts
Promise.race([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) =>
    setTimeout(() => reject(new Error('에러 발생!')), 2000)
  ),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000)),
]).then(alert) // 1
```

메서드 명이 race인 이유가 가장 먼저 도착한 프로미스가 경주에서 우승하는 것과 같다고 생각하면 이해하기 쉽습니다.

#### Promise.resolve/reject

Promise.resolve(result)는 결과값이 항상 result인 fulfilled 상태의 프로미스를 반환합니다.
Promise.reject(error)는 에러 error를 담은 rejected 상태의 프로미스를 반환합니다.

### Q. then/catch/finally가 비동기로 작동하는 원리를 설명해주세요.

프로미스가 이행된 순간 바로 .then/catch/finally에 등록된 함수가 실행되는건 아니다.

프로미스가 이행되면 .then/catch/finally에 등록된 함수가 마이크로태스크 큐에 들어간다. 마이크로태스크 큐는 이벤트 루프에 의해 콜 스택이 비워졌을 때 실행된다.

즉, 프로미스가 이행되면 마이크로태스크 큐에 각 핸들러가 enqueue되는 것이다.

### async/await은 무엇인가요?

#### async

async/await은 ES6에서 등장한 프로미스를 편하게 사용하기 위한 문법이다.

async는 항상 함수 앞에 선언되며 async가 선언된 함수는 항상 프로미스를 반환한다.

함수가 프로미스가 아닌 값을 반환하더라도 그 반환값을 프로미스로 감싸 이행된 프로미스를 반환한다.

```ts
async function f() {
  return 1
}

f().then(alert) // 1
```

#### await

await은 프로미스 앞에서 사용되며 프로미스가 처리될 때까지 함수 실행을 일시 중지하고, 프로미스가 처리되면 처리 결과를 반환한다.

여기서 실행을 중지한다는 의미는 함수 내부에서 await 이후의 실행을 중지하고 다른 작업을 수행한다는 의미입니다. 따라서 CPU 리소스가 낭비되지는 않는다.

### Q. then/catch/finally와 async/await중 어느것을 사용할 것인가요?

then/catch/finally 프로미스 체이닝은 함수가 옆으로 길어지는 콜백 지옥은 해결해주지만 코드가 아래로 길어지기도 한다

async/await은 프로미스 체이닝을 사용할 때와 같이 코드가 아래로 길어지는 것을 해결해주며 좀 더 직관적인 코드를 작성할 수 있기 때문에 async/await 사용을 선호한다.

## reference

https://ko.javascript.info/promise-api
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise
