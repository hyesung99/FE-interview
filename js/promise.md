### callback은 무엇인가요?

callback은 매개변수로 함수 객체를 전달해서 호출 함수 내에서 매개변수 함수를 실행하는 것을 말한다.

콜백은 비동기 처리에서 사용될 수 있다

무언가를 비동기적으로 수행하는 함수는 함수 내 동작이 모두 처리된 후 실행되어야 하는 함수를 인자로 받아서 처리가 완료된 후에 실행되도록 할 수 있다.

### callback의 문제점은 뭔가요?

callback을 중첩 호출하기 위해서는 함수를 중첩해서 호출해야 한다.
이는 코드의 가독성을 매우 떨어뜨리며 관리하기 힘들게 만든다.
이는 콜백 지옥이라는 문제로 이어진다.

### 콜백 지옥문제는 어떻게 해결할 수 있나요?

Promise를 사용하면 콜백 지옥 문제를 해결할 수 있다.

### Promise는 무엇인가요?

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

### Promise 어떻게 비동기 처리를 하나요?

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

#### finally :

finally는 프로미스 이행 결과와 상관없이 실행된다.

### 이것으로 어떻게 콜백 지옥을 해결할 수 있나요?

기존 콜백 지옥을 유발하는 콜백함수가 프로미스 객체를 반환하게 하면 된다.

기존 스스로 함수 내부에서 자신의 실행을 마무리하면 콜백을 실행하는 구조에서 실행이 마무리 되면 프로미스 생성자 내부에서 resolve를 호출하면 된다.

그리고 함수 외부에서는 then을 사용하여 콜백을 등록하면 된다.

만약 비동기 호출을 중첩하여 호출해야 한다면 프로미스 체이닝을 사용하면 된다.

### 프로미스 체이닝이 뭔가요?

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

### 프로미스 체이닝은 어떻게 가능한가요?

then메서드가 프로미스를 반환하기 때문이다. then메서드에서 그대로 값을 반환하면 Promise.resolve(value)를 반환하는 것과 같다.
