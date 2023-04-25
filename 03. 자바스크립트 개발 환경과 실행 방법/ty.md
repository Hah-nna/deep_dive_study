# 브라우저와 Node.js 존재 목적

브라우저는 HTML,css,자바스크립트를 실행하여 웹페이지를 화면에 렌더링하는 것이 주된 목적이지만, Node.js는 서버 개발 호나경을 제공하는 것이 주된 목적이다.

브라우저는 ECMAScript와 DOM, BOM, Canvas, XMLHttpRequest, Fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web work와 같은 클라이언트 사이드 Web API를 지원한다. Node.js는 클라이언트 사이드 Web API는 지원하지 않고 ECMAScript와 Node.js 고유의 API를 지원한다.

# 1. 웹브라우저.

크롬브라우저의 v8 자바스크립 엔진은 Node.js에서도 사용하고 있다.

- 1.1 개발자도구
- 1.2 콘솔
- 1.3 HTML에 포함된 자바스크립트를 브라우저에서 실행
- 1.4 디버깅

# 2. Node.js

프로젝트 규모가 커짐에 따라 React, jQuery와 같은 외부 라이브러리를 도입하거나 babel, webpack, ESlint 등 여러 가지 도구를 사용해야 할 필요가 있다. 이때 Node.js와 npm이 필요하다.

- 2.1 Node.js 와 npm
  - 브라우저에서만 동작하던 자바스크립트를 브라우저 이외의 환경에서 동작시킬 수 있는 자바스크립트 실행환경이 Node.js(자바스크립트 런타임 환경)<br>
  - Node.js는 주로 서버 사이드 애플리케이션 개발에 사용되며 이에 필요한 모듈, 파일 시스템, HTTP 등 빌트인 API를 제공한다. Node.js는 데이터를 실시간 처리하여 빈번한 I/O가 발생하는 SPA(Single Page Application)에 적합하다. 하지만 CPU 사용률이 높은 애플리케이션에는 권장하지 않는다. <br>
  - Node.js는 백엔드 영역에서 애플리케이션 개발뿐만 아니라 프런트엔드 영역의 다양한 도구나 라이브러리 도 Node.js 환경에서 동작한다. 따라서 Node.js는 프런트엔드 모던 자바스크립트 개발에 필수적인 환경이라고 할 수 있다.
  - **npm (node package manager)** 은 자바스크립트 패키지 매니저이다. Node.js에서 사용할 수 있는 모듈들을 패키지화하여 모아둔 저장소 역할과 패키지 설치 및 관리를 한 CLI(Command Line interface)를 제공한다. 자신이 작성한 패키지를 공개할 수도 있고 필요한 패키지를 검색하여 재상용 할 수도 있다. <br>
- 2.2 Node.js 설치
  - Node.js 웹사이트 에서 설치.(npm 도 동시에 설치된다.)<br>
  - 설치 완료후 터미널에 node -v, npm -v 버전확인. <br>
- 2.3 Node.js REPL
  - PEPL(Real Eval Print Loop)은 간단한 코드를 직접 실행해 결과를 확인해 볼 수 있다. 
```js
    node index.js // node 해당파일명.js .js는 생략가능
```