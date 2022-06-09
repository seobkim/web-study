# React.js 
- Node.js라는 자바스크립트 런타임환경을 이용 => 브라우저 밖에서도 자바스크립트를 컴파일하고 실행 가능

# Node.js
- Node.js가 등장하기 전까지 자바스크립트는 브라우저 내에서만 실행할 수 있었다.
- 자바스크립트를 브라우저 밖에서 실행할 수 있다는 것은 자바스크립트를 클라이언트 언어뿐만 아니라 서버 언어로도 사용할 수 있다는 뜻이다

# NPM
- NPM(Node Package Manager)은 Node.js의 패키지 관리 시스템


# SPA (Single Page Application) 
- 대중적인 SPA 라이브러리/프레임워크 -> React, Angular, Vue

# CSR / SSR
- CSR (Client Side Rendering) : 서버에서 새 HTML 페이지를 요청하지 않고 자바스크립트가 동적으로 HTML을 재구성해 만드는 클라이언트 애플리케이션을 싱글 페이지 애플리케이션이라고 하고
이 렌더링 과정을 클라이언트-사이드 렌더링이라고 한다
- SSR (Server Side Rendering) : 위와 반대 

# JSX
- React가 한 파일에서 HTML과 JS를 함꼐 사용하려고 확장한 자바스크립트 문법이다
- JSX 문법은 Babel이라는 라이브러리가 빌드 시 자바스크립트로 변환


# 리액트의 Rendering
- 리액트는 브라우저에 보이는 HTML DOM 트리의 다른 버전인 ReactDom(Virtual Dom)을 갖고 있다. 어떤 이유에서든 컴포넌트의 상태가 변하면 ReactDom은 이를 감지하고 변경된 부분의 HTML을 바꿔준다.
- HTML이 업데이트되면 변경된 결과를 확인할 수 있다.

- 렌더링이 맨 처음 일어나는 순간, 즉 ReactDom 트리가 존재하지 않는 상태에서 리액트가 처음으로 각 컴포넌트의 render() 함수를 콜해 자신의 DOM 트리를 구성하는 과정을 마운팅(mounting)이라고 한다.
- 마운팅 과정에서 생성자와 render() 함수를 부르는데 마운팅을 마친 후 바로 부르는 함수가 componentDidMount라는 함수다
- 컴포넌트가 렌더링 되었을 시 componentDidMount함수에 백엔드 API 콜 부분을 구현해야 한다
