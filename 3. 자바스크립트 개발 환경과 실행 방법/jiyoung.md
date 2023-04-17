# 3 Hello world

# 자바스크립트 개발 환경과 실행 방법

기본적으로 브라우저와, node.js는 자바스크립트 엔진을 내장하고 있어 자바스크립트를 실행할 수 있다.

- 브라우저: HTML, CSS, 자바시크립트를 실행하여 웹 페이지를 화면에 렌더링하는 것이 목적
  - ECMAScript, DOM, BOM, Canvas, XMLHttpRequest, Fetch, RequestAnimationFrame, SVG, Web Storage, Web Component, Web Worker와 같은 클라이언트 사이드 Web API 지원
- Node.js: 서버 개발 환경을 제공하는 것이 목적
  - ECMAScript와 Node.js 고유의 API 지원

# 웹 브라우저

생략

## 개발자 도구

크롬 브라우저가 제공하는 개발자 도구는 F12 또는 command + option + I 로 오픈할 수 있다.
개발자 도구에서 제공하는 도구의 기능
Elements: 로딩된 웹 페이지의 DOM과 CSS를 편집하여 렌더링 된 뷰를 확인할 수 있다. (저장 불가능)
Console: 로딩된 웹 페이지이 에러를 확인하거나 자바스크립트 소스코드에 포함시킨 console.log 메소드 결과를 확인 할 수 있다. (자바스크립트 코드 실행 중 에러가 발생하면 에러 내용이 콘솔에 출력)
Source: 로딩된 웹 페이지의 자바스크립트 코드를 디버깅 할 수 있다.
Network: 로딩된 웹 페이지에 관한 네트워크 요청(request) 정보와 퍼포먼스틑 확인할 수 있다.
Application: 웹 스토리지, 세션, 쿠키를 확인하고 관리할 수 있다.

## 콘솔

생략

## HTML에 포함된 자바스크립트를 브라우저에서 실행

브라우저에 HTML파일을 로드하면 script 태그에 포함한 자바스크립트 코드를 실행한다.

## 디버깅

생략

# Node.js

클라이언트 사이드(웹 브라우저)에서 React, jQuery와 같은 외부 라이브러리나 Babel, Webpack,ESlint 등 여러가지 도구를 사용할 때 Node.js와 npm이 필요하다.

## Node.js와 npm 소개

Node.js는 Chrome V8 자바스크립트 엔진으로 빌드된 **자바스크립트 런타인 환경** 이다.
다시 말해, Node.js는 브라우저에서만 동작하던 자바스크립트를 브라우저 이외의 환경에서 동작시킬 수 있는 자바스크립트 실행 환경이다.

npm(node package manager)은 자바스크립트 패키지 매니저이다.
Node.js에서 사용할 수 있는 모듈들을 패키지화하여 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 CLI(command line interface)를 제공한다.

## Node.js 설치

http://node.js.org 에서 설치할 수 있다.

## Node.js REPL

REPL(Read Eval Print Loop)은 대부분의 언어가 제공하는 가상환경으로 간단한 코드를 직접 실행하고 결과를 확인할 수 있다.

# 비주얼 스튜디오 코드

생략
