※ 1. 웹 브라우저, 1.1 개발자도구, 1.2 콘솔, 2.1 Node.js와 npm 소개, 2.2 Node.js 설치, 2.3 Node.js REPL, 3. 비주얼 스튜디오 코드 부분은 생략했습니다

모든 브라우저는 자바스크립트를 해석하고 실행할 수 있는 자바스크립트 엔진을 내장하고 있음
브라우저뿐만 아니라 Node.js도 자바스크립트 엔진을 내장하고 있다
그런데 브라우저와 Node.js는 존재 목적이 다름
브라우저는 HTML, CSS, JS를 실행하여 웹 페이지를 화면에 렌더링하는 게 주된 목적임
Node.js는 서버개발 활경을 제공하는 것이 목적임
따라서 브라우저와 Node.js 둘 다 js의 코어인 ECMAScript를 실행할 수 있지만 브라우저와 Node.js에서 ECMAScript 이외에 추가적으로 제공하는 기능은 호환되지 않음
ex) 브라우저에서 DOM API 제공 / Node.js에서는 제공 x
브라우저 File 시스템 제공 x(fileReader로 읽는 건 가능) / Node.js에서는 File 시스템 제공(파일의 수정, 삭제 가능)

즉, 브라우저는 클라이언트 사이드 Web API을 지원하고 Node.js는 ECMAScript와 Node.js 고유의 API를 지원함

### HTML에 포함된 자바스크립트를 브라우저에서 실행

브라우저는 HTML 파일을 로드하면 script 태그에 포함한 자바스크립트 코드를 실행함

### 디버깅

브라우저마다 개발자 도구가 있음
이를 활용해 디버깅을 하는 데 사용함
크롬의 경우 [콘솔사용법](https://developer.chrome.com/docs/devtools/)와
[크롬데브툴js디버깅](https://developer.chrome.com/docs/devtools/)을 참고하자

# Node.js

프로젝트의 규모가 커짐에 따라 React, jQuery와 같은 외부 라이브러리를 도입하거나 Babel, Webpack, ESlint 등 여러 가지 도구를 사용해야 할 필요성 생김
이때 Node.js와 npm이 필요함

### Node.js와 npm 소개

2009년 Ryan Dahl이 발표한 Node.js는 Chrome V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임 환경임
즉, 브라우저에서만 동작하던 자바스크립트를 브라우저 이외의 환경에서 동작시킬 수 있는 자바스크립트 실행 환경이 Node.js임

Node.js는 주로 서버 사이드 애플리케이션 개발에 사용되며 이에 필요한 모듈, 파일 시스템, HTTP 등 빌트인 API를 제공함
Node.js는 SPA에 적합하나 CPU 사용률이 높은 애플리케이션에는 권장하지 않음

npm(node package manager) === 자바스크립트 패키지 매니저
Node.js에서 사용할 수 있는 모듈들을 패키지화하여 모아둔 저장소 역할+ 패키지 설치 및 관리를 위한 CLI(Command line interface)를 제공함
