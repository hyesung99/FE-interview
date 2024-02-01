## Q. 코드 스플리팅이 뭔가요?

코드 스플리팅은 코드 분할이라고도 하며, 애플리케이션의 JS코드를 분할하여 번들링하는 것을 말합니다.

## Q. 코드 스플리팅을 하는 이유는 뭔가요?

리액트를 예시로 들겠습니다.
리액트 환경의 SPA방식은 애플리케이션의 모든 JS코드를 한번에 다운로드 받습니다.
이는 SPA의 장점이자 단점이 된다.
페이지 이동시 서버에서 새로운 HTML을 받지 않고 필요한 데이터만 ajax를 통해 받아오면 되기 때문에 페이지간 이동이 빠르다.
하지만 초기 모든 JS코드를 다운로드 받아야 하기 떄문에 초기 로딩속도가 느리다는 단점이 있다.
초기 로딩속도는 프론트엔드 성능의 중요한 지표이기 때문에 이를 해결하기 위해 코드 스플리팅을 사용한다.

코드를 분할하여 필요한 코드만 다운로드 받는 방식으로 초기 로딩속도를 줄인다.

## Q. 코드 스플리팅은 어떻게 할 수 있나요?

React.lazy 함수를 사용하면 동적 import를 사용해서 컴포넌트를 렌더링 할 수 있습니다.

## Q. 코드를 분할하는 기준이 있나요?

코드 분할을 어느 곳에 도입할지 결정하는 것은 조금 까다롭다.
사용자 경험을 헤치지 않으면서 어느 지점에서 코드를 받아올지 결정하는 것이 어렵기 때문다.

명료하고 효과적인 방법은 라우트에서 사용하는 것이다.
React.Router라이브러리를 사용하는 예시이다.

```ts
import React, { Suspense, lazy } from 'react'
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom'

const Home = lazy(() => import('./routes/Home'))
const About = lazy(() => import('./routes/About'))

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path='/' element={<Home />} />
        <Route path='/about' element={<About />} />
      </Routes>
    </Suspense>
  </Router>
)
```

## Q. React.lazy는 무엇인가요?

React.lazy는 동적 import를 도와주는 함수다.
React.lazy의 인자로 특정 컴포넌트를 import하는 콜백을 넘겨주면
해당 컴포넌트가 렌더링 될 때 비동기적으로 컴포넌트를 불러온다.

## Q. suspense는 무엇인가요?

suspense는 자식 컴포넌트가 비동기 처리를 하는 동안 로딩 상태를 보여주는 컴포넌트다. React.lazy를 사용한 컴포넌트는 비동기적으로 js파일을 불러오기 때문에 React.lazy 로딩 상태를 보여주기 위해 suspense와 함께 사용한다.

suspense의 fallback을 통해 로딩 중일 때 어떤 컴포넌트를 보여줄 지 설정할 수 있다. 여기에는 스피너나 스켈레톤 UI등이 들어갈 수 있다.
