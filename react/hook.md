클래스형 컴포넌트의 설명으로 시작해서 클래스형 컴포넌트의 문제점. 해결 방법으로 나온 함수형 컴포넌트와 훅을 설명해보고 동작 방식 까지 깊게 알아보자.

자세한 훅 사용법은 담지 않음. 사용법은 공식문서 참조!

### 레퍼런스

https://ko.legacy.reactjs.org/docs/hooks-intro.html#gradual-adoption-strategy
https://react.vlpt.us/basic/25-lifecycle.html

<hr/>

_우선 클래스 컴포넌트와 라이프 사이클 메서드를 이해해야 훅의 등장배경과 목적을 알 수 있다._

### 리액트의 라이프 사이클 메서드을 설명해주세요.

![Alt text](life_cycle.png)

### 함수형 컴포넌트와 클래스형 컴포넌트는 어떤 차이가 있나요?

### 그렇다면 왜 요즘에는 함수형 컴포넌트를 많이 쓰나요?

- js에서 this사용의 혼란
- 각 생명주기에 관련없는 메서드들이 들어가기도 함. 무결성을 쉽게 해침.
- Class 컴포넌트의 구별, 각 요소의 사용 타이밍 등은 숙련된 React 개발자 사이에서도 의견이 일치하지 않았다.

### 훅이 뭔가요?

Hook은 함수 컴포넌트에서 React state와 생명주기(lifecycle)를 “연동(hook into)“할 수 있게 해주는 함수입니다. Hook은 class 안에서는 동작하지 않습니다. 대신 class 없이 React를 사용할 수 있게 해주는 것입니다. 훅의 등장으로 인해 class 컴포넌트의 한계를 극복할 수 있게 되었습니다.

### 훅의 동작 순서는 어떻게 되나요?

![Alt text](hook_flow.png)

### setState는 어떻게 동작하나요?

클래스 컴포넌트에서는 this.setState를 호출한다.
this.setState는 state를 병합한다
ex)

```js
this.state = { name: 'jane', age: 20 }
this.setState({ age: 21 })
console.log(this.state) // {name: 'jane', age: 21}
```

함수형 컴포넌트에서는 state를 다룰 때 useState를 사용한다.
useState는 상태를 병합하는 것이 아닌 대체한다

ex)

```js
const [state, setState] = useState({ name: 'jane', age: 20 })
setState({ age: 21 })
console.log(state) // {age: 21}
```
