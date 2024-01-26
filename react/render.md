## 렌더링이란 무엇인가요?

렌더링이란 React가 현재 props와 state를 기반으로 UI를 만들어내는 과정을 뜻한다.

## 렌더링이 일어나는 과정을 설명해주세요.

#### 엘리먼트 렌더링

렌더링이 일어나면 React는 컴포넌트 트리의 루트에서부터 아래쪽으로 내려가며 업데이트가 필요한 컴포넌트를 찾고 그 컴포넌트의 결과물을 저장한다.
여기서 컴포넌트 실행의 결과는 JSX 구문으로 작성되며 babel은 컴파일 시 React.createElement()로 변환한다.

createElement는 일반적인 JS 객체인 React 엘리먼트를 반환한다. 이 엘리먼트는 UI구조를 어떻게 표현할지에 대한 정보를 가지고 있다.

#### DOM에 엘리면트 렌더링

```ts
// 다음과 같은 JSX 문법이:
return <SomeComponent a={42} b="testing">Text here</SomeComponent>

// 이런 식의 호출로 변환됩니다:
return React.createElement(SomeComponent, {a: 42, b: "testing"}, "Text Here")

// 그렇게해서 이런 엘리먼트 객체가 됩니다:
{type: SomeComponent, props: {a: 42, b: "testing"}, children: ["Text Here"]}
```
