클래스형 컴포넌트의 설명으로 시작해서 클래스형 컴포넌트의 문제점. 해결 방법으로 나온 함수형 컴포넌트와 훅을 설명해보고 동작 방식 까지 깊게 알아보자.

자세한 훅 사용법은 담지 않음. 사용법은 공식문서 참조!

<hr/>

_우선 클래스 컴포넌트와 라이프 사이클 메서드를 이해해야 훅의 등장배경과 목적을 알 수 있다._

## 리액트의 라이프 사이클 메서드을 설명해주세요.

![Alt text](life_cycle.png)

1. 컴포넌트가 최초로 생성될 때 라이프 사이클
   - constructor
     클래스 생성자 메서드
   - getDerivedStateFromProps
     prop을 state에 할당
   - render
     컴포넌트를 렌더링
   - componentDidMount
     컴포넌트의 렌더링이 끝나고 호출되는 메서드, DOM의 속성을 읽거나 변경 가능, ajax 요청도 여기서 일어난다.

</br>
  
2. 컴포넌트가 업데이트 되었을 때 라이프 사이클
   * getDerivedStateFromProps (
    prop을 state에 할당
   * shouldComponentUpdate
    컴포넌트가 리렌더링 할지 말지를 결정하는 메서드
   * render
   * getSnapshotBeforeUpdate
   컴포넌트에 변화가 일어나기 직전의 DOM 상태를 가져와서 특정 값을 반환하면 그 다음 발생하게 되는 componentDidUpdate 함수에서 받아와서 사용을 할 수 있다 
   * componentDidUpdate 
   리렌더링이 마치고, 화면에 우리가 원하는 변화가 모두 반영되고 난 뒤 호출되는 메서드. 3번째 파라미터로 getSnapshotBeforeUpdate 에서 반환한 값을 조회 할 수 있다.

</br>
3. 컴포넌트가 제거 될 때 라이프 사이클
   * componentWillUnmount
   포넌트가 화면에서 사라지기 직전에 호출, 주로 DOM에 바인딩한 이벤트 제거

## 함수형 컴포넌트와 클래스형 컴포넌트는 어떤 차이가 있나요?

1. this 키워드 사용의 차이
   클래스형 컴포넌트에서는 state,prop에 접근할 때 this키워드를 사용하여 접근한다. (this.state, this.props). 함수형 컴포넌트에서는 this키워드를 사용할 수 없다. state, prop에 접근할 때 추가적인 키워드 없이 바로 접근이 가능하다.

2. 생명주기 메서드 사용의 차이
   클래스형 컴포넌트에서는 생명주기 메서드를 사용하여 특정 시점에 작업을 실행한다. componentDidMount, componentDidUpdate, componentWillUnmount 등이 있다. 함수형 컴포넌트에서는 생명주기 메서드를 사용할 수 없다. 대신 useEffect라는 함수를 사용하여 특정 시점에 작업을 처리한다. useEffect처럼 생명주기 메서드 다루는 것을 포함하여 리액트의 기능을 효과적으로 다룰 수 있게 해주는 함수를 Hook이라고 한다.

3. 상태 관리의 차이
   this키워드로 접근하여 this.setState, this.state를 사용하여 상태를 관리한다. 함수형 컴포넌트에서는 useState라는 훅을 사용하여 상태를 관리한다.

## 그렇다면 왜 요즘에는 함수형 컴포넌트를 많이 쓰나요?

클래스형 컴포넌트는 컴포넌트를 하나의 객체로 다루는 것과 비슷하다 만약 내부 상태의 변화가 생긴다면 자신의 메서드인 render()함수를 실행시킬 것이다. 따라서 특정 시점마다 컴포넌트는 다른 결과물을 반환할 것이다. 하지만 이런 특성은 어느 시점에 어떤 결과를 받게 될지 예상하기 힘들게 만든다.

함수형 컴포넌트에서는 이 문제점을 불변성 특성을 통해 해결한다. 함수형 컴포넌트는 결국 리액트 컴포넌트를 반환하는 함수일 뿐이다. 함수형 컴포넌트는 호출과 반환으로 이루어지는 하나의 흐름을 가지기 때문에 중간에 생길 의도치 않은 변화를 방지한다. 함수형 컴포넌트의 내용을 바꾸기 위해서는 매개변수를 변경하여 다시 함수형 컴포넌트를 실행시켜 새로운 리액트 컴포넌트를 반환받으면 되는 것이다.

따라서 함수형 컴포넌트를 사용하면 어떤 값이 반환 될지 예상하기 쉽고, 컴포넌트의 무결성을 쉽게 유지할 수 있다.

하지만 초기에 함수형 컴포넌트는 사용되지 않고 클래스형 컴포넌트가 주로 사용됐다. 함수형 컴포넌트는 기본적으로 상태를 관리할 수 없고 외부에서 주입받을 수 밖에 없다. 또한 컴포넌트의 life cycle에 따라 특정 작업을 처리할 수 없었다. 따라서 클래스형 컴포넌트를 계속 사용했다.

하지만 클래스 형 위에서 언급한 결과 예측이 힘들다는 점 이외에 컴포넌트에는 몇가지 문제점이 더 있었다.

- js에서 this사용의 혼란 : js는 고전적인 객체지향 언어들과 다르게 this가 상황에 따라 다르게 바인딩 된다.
- 각 생명주기에 관련없는 메서드들이 들어가기도 함. 무결성을 쉽게 해침. 또한 각 생명주기에 어떤 내용이 들어가야하는지 개발자 사이에서 의견이 일치하지 않았다.

따라서 React 16.8 버전에서 함수형 컴포넌트를 위한 Hook이 탄생하게 되었다. 이제 Hook을 통해 함수형 컴포넌트에서도 상태를 관리하고, 생명주기에 따른 작업을 처리할 수 있게 되었다.

이후에는

## 훅이 뭔가요?

예를들어 useEffect 훅은 함수 컴포넌트에서 React state와 생명주기(lifecycle)를 “연동(hook into)“할 수 있게 해주는 함수이다. 즉 라이프 사이클 메서드를 쉽게 사용할 수 있도록 만든 함수. 이 밖에도 useContext, useMemo, useCallback, useRef, useReducer, useLayoutEffect등 함수형 컴포넌트에서 사용할 수 있는 여러가지 도구를 제공한다. 넓게 보면 함수형 컴포넌트에서 리액트의 기능을 사용할 수 있게 해주는 함수라고 할 수 있다.

## 훅의 동작 순서는 어떻게 되나요?

![Alt text](hook_flow.png)

## useState는 뭔가요?

함수형 컴포넌트에서 상태를 관리할 수 있게 만들어주는 훅이다.

## useState는 어떻게 동작하나요?

useState는 state와 setState를 반환한다.

```tsx
const [state, setState] = useState(initialState)
```

여기서 주목할 점은 state는 const로 선언된다는 것이다. 즉 state는 한번의 함수 실행에서 불변성을 유지한다. 하지만 위에서 말했듯이 함수형 컴포넌트에서도 상태를 관리해야 한다.
따라서 state는 closure로 관리된다.

```tsx
// Example 2
const MyReact = (function () {
  let _val // hold our state in module scope
  return {
    render(Component) {
      const Comp = Component()
      Comp.render()
      return Comp
    },
    useState(initialValue) {
      _val = _val || initialValue // assign anew every run
      function setState(newVal) {
        _val = newVal
      }
      return [_val, setState]
    },
  }
})()
```

위 코드처럼 함수 컴포넌트의 실행이 끝나도 \_val에 의해 상태가 메모리에 유지된다.
setState를 실행하면 \_val에 새로운 값을 할당한다.

이를 통해 함수형 컴포넌트 내부에서 불변성을 유지하며 상태를 관리하게 된다.

실제 리액트에서는 각 컴포넌트와 관련된 메모리 셀 내부에 상태를 저장한다. 메모리 셀은 js객체이다.

## useState는 여러 컴포넌트에서 실행할텐데, 어떻게 각 컴포넌트와 상태를 연결하나요?

리액트는 현재 렌더링된 컴포넌트를 추적합니다. 위에서 말했듯이 리액트 state는 메모리 셀에 저장되어 있다. useState같은 훅을 호출하면 현재 셀을 읽은 뒤 포인터를 다음 셀로 이동시킨다. 이렇게 useState()는 독립적은 state를 가진다.

## setState를 한 컴포넌트에서 호출할 때마다 리렌더링이 일어나나요?

```tsx
function Counter() {
  const [count, setCount] = useState(0)
  setCount(count + 1)
  setCount(count + 1)
  setCount(count + 1)
}
```

기본적으로 함수형 컴포넌트는 setState()를 실행하면 컴포넌트를 re-render하여 변경된 상태가 적용된 컴포넌트를 반환한다. 위 예제에서 그렇다면 setCount()를 세번 실행했으니 3번 리렌더링이 일어날 것이라고 예측할 수 있다.

하지만 리액트는 이런 경우를 최적화하여 한번만 리렌더링이 일어나도록 한다. 이를 배칭이라고 한다.

## 배칭(batching)이란 무엇인가요?

배칭은 리액트가 더 나은 성능을 위해 여러 setState()를 한번에 처리하는 것을 말한다.
배칭을 사용하여 여러번의 setState()호출을 묶어 한번의 리렌더링으로 처리한다.

또한 모든 상태 변경을 호출 한 뒤 리렌더링을 하기 때문에 반만 완료된 상태로 리렌더링 되는 것을 방지한다.

리액트 18 이전까지, React 이벤트 핸들러 내부에서 발생하는 업데이트만 배칭을 하였다. Promise, setTimeout, native 이벤트 핸들러, 그리고 여타 모든 이벤트 내부에서 발생하는 업데이트들은 React에서 배칭되지 않았다.

React 18의 createRoot를 통해, 모든 업데이트들은 어디서 왔는가와 무관하게 자동으로 배칭되게 된다.

## 배칭은 어떻게 동작하나요? ( 와 진짜 여기까지 물어볼까? )

### Reference

https://ko.legacy.reactjs.org/docs/hooks-faq.html#how-does-react-associate-hook-calls-with-components
https://ko.legacy.reactjs.org/docs/hooks-intro.html#gradual-adoption-strategy
https://github.com/facebook/react
