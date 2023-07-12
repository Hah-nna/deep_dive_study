## 1. SPA (Single Page Application)

단일 어플리케이션은 모던 웹의 패러다임임. SPA는 기본적으로 단일 페이지로 구성되며 기존의 서버 사이드 렌더링과 비교할 때 배포가 간단하여 네이티브 앱과 유사한 사용자 경험을 제공할 수 있다는 장점이 있음

link tag를 사용하는 전통적인 화면 전환 방식은 새로운 페이지 요청 시마다 정적 리소스가 다운로드 되고 전체 페이지를 다시 렌더링하는 방식을 사용하므로 새로고침이 발생되어 사용성이 좋지 않음. 그리고 변경이 필요없는 부분까지 포함하여 전체 페이지를 갱신하므로 비효율적임

SPA는 기본적으로 웹 어플리케이션에 필요한 모든 정적 리소스를 최초 접근시 단 한번만 다운로드 함. 이후 새로운 페이지 요청 시 페이지 갱신에 필요한 부분만을 JSON으로 전달받아 페이지를 갱신하므로 전체적인 트래픽을 감소시킬 수 있고 전체 페이지를 다시 렌더링하지 않고 변경되는 부분만을 갱신하므로 새로고침이 발생되지 않아 네이티브 앱과 유사한 사용자 경험을 제공할 수 있음

모바일 사용이 데스크톱을 넘어선 현재, 트래픽 감소와 속도, 사용성, 반응성의 향상은 매우 중요한 이슈임. SPA의 핵심가치는 **사용자 경험(UX) 향상**에 있으며 부가적으로 애플리케이션 속도의 향상도 기대할 수 있어서 모바일 퍼스트 전략에 부합함.

모든 소프트웨어 아키텍처에는 트레이드오프가 존재하며 모든 애플리케이션에 적합한 은탄환은 없듯이 SPA 또한 구조적인 단점을 가지고 있음. 대표적인 단점은 다음과 같음

- 초기 구동 속도
  SPA는 웹 어플리케이션에 필요한 모든 정적 리소스를 최초 접근시 단 한번 다운로드하기 때문에 초기 구동 속도가 상대적으로 느림. 하지만 SPA는 웹페이지보다는 어플리케이션에 적합한 기술이므로 트래픽 감소와 속도, 사용성, 반응성의 향상 등의 장점을 생각한다면 결정적인 단점이라고는 할 수 없음

- SEO 이슈
  SPA는 일반적으로 서버 사이드 렌더링 방식이 아닌 자바스크립트 기반 비동기 모델의 클라이언트 사이드 렌더링 방식으로 동작함. 클라이언트 사이드 렌더링은 일반적으로 데이터 패치 요청을 통해 서버로부터 응답받아 뷰를 동적으로 생성하는데 이때 브라우저 주소창의 URL이 변경되지 않음. 이는 사용자 방문 히스토리를 관리할 수 없음을 의미하며 SEO 이슈의 발생 원인이기도 함. SPA의 SEO이슈는 언제나 단점으로 부각되어왔음. SPA는 정보 제공을 위한 웹페이지보다는 어플리케이션에 적합한 기술이므로 SEO 이슈는 심각한 문제로 취급할 수 없다고 생각할 수도 있지만 블로그와 같이 어플리케이션의 경우 SEO는 무시할 수 없는 중요한 의미를 가짐. Angular나 react 등의 SPA 프레임워크는 서버 사이드 렌더링을 지원하는 기능이 이미 존재하고 크롬 등의 모던 브라우저는 SPA의 SEO문제를 해결하고 있는 것으로 알려져 있음

## 2. Routing

라우팅이란 출발지에서 목적지까지의 경로를 결정하는 기능임. 애플리케이션의 라우팅은 사용자가 태스크를 수행하기 위해 어떤 화면에서 다른 화면으로 화면을 전환하는 내비게이션을 관리하기 위한 기능을 의미함. 일반적으로 라우팅은 사용자가 요청하 URL 또는 이벤트를 해석하고 새로운 페이지로 전환하기 위해 필요한 데이터를 서버에 요청하고 페이지를 전환하는 일련의 행위를 말함

브라우저가 화면을 전환하는 경우는 다음과 같음

1. 브라우저의 주소창에 URL을 입력하면 해당 페이지로 이동함
2. 웹페이지의 링크(a 태그)를 클릭하면 해당 페이지로 이동
3. 브라우저의 뒤로가기 또는 앞으로 가기 버튼을 클릭하면 사용자 방문 기록의 뒤 또는 앞으로 이동함. **히스토리를 관리하기 위해서는 각 페이지는 브라우저의 주소창에서 구별할 수 있는 유일한 URL을 소유해야함**

## 3. SPA와 Routing

예제 클론와 실행 방법은 생략함

### 3.1 전통적 링크 방식

전통적 링크 방식은 link tag로 동작하는 기본적인 웹페이지의 동작 방식임
link tag를 클릭하면 href 어트리뷰트 값인 리소스 경로가 URL의 path에 추가되어 주소창에 나타나고 해당 리소스를 서버에 요청함

<div align="center">
<img src="https://poiemaweb.com/img/uri.png" width="50%" height="50%">
</div>

이떄 서버는 html로 화면을 표시하는데 부족함이 없는 완전한 리소스를 클라이언트에 응답함. 이를 서버 사이드 렌더링(SSR)이라 함
브라우저는 서버가 응답한 html을 응답받아 렌더링함. 이때 응답받은 html로 전체 페이지를 다시 렌더링하게 되므로 새로고침이 발생함

<div align="center">
<img src="https://poiemaweb.com/img/traditional-webpage-lifecycle.png" width="50%" height="50%">
</div>

이 방식은 자바스크립트의 도움 없이 응답받은 html 만으로 렌더링이 가능하며 각 페이지마자 고유의 URL이 존재하므로 히스토리 관리 및 SEO 대응에 아무런 문제가 없음. 하지만 요청마다 중복된 리소스를 응답받아야 하며 전체 페이지를 다시 렌더링하는 과정에서 새로고침이 발생하여 사용성이 좋지 않은 단점이 있음

### 3.2 ajax 방식

전통적 링크 방식은 현재 페이지에서 수신된 html로 화면을 전환하는 과정에서 전체 페이지를 새롭게 렌더링하게 되므로 새로고침이 발생함. 간단한 웹페이지라면 문제될 것이 없겠지만 복잡한 웹페이지의 경우 요청마다 중복된 HTML과 css, javascript를 매번 다운로드 해야하므로 속도 저하의 요인이 됨.

전통적 링크 방식의 단점을 보완하기 위해 등장한 것이 바로 ajax임. ajax는 자바스크립트를 이용해 비동기적으로 서버와 브라우저가 데이터를 교환할 수 있는 통신 방식을 의미함

<div align="center">
<img src="https://poiemaweb.com/img/ajax-webpage-lifecycle.png" width="50%" height="50%">
</div>

서버로부터 웹페이지가 응답되면 화면 전체를 새롭게 렌더링해야 하는데 페이지 일부만 갱신하고도 동일한 효과를 볼 수 있도록 하는 것이 ajax임

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>SPA-Router - ajax</title>
    <link rel="stylesheet" href="css/style.css" />
    <script type="module" src="js/index.js"></script>
  </head>
  <body>
    <nav>
      <ul id="navigation">
        <li><a href="/">Home</a></li>
        <li><a href="/service">Service</a></li>
        <li><a href="/about">About</a></li>
      </ul>
    </nav>
    <div id="root">Loading...</div>
  </body>
</html>
```

위 예제를 살펴보면 link tag의 href 어트리뷰트에 path를 사용하고 있음. 따라서 내비게이션이 클릭되면 path가 추가된 URL이 서버로 요청됨. 하지만 ajax 방식은 내비게이션 클릭 이벤트를 캐치하고 preventDefault 메서드를 사용해 서버로의 요청을 방지한 이후 href 어트리뷰트에 path을 사용하여 ajax 요청을 하는 방식임

그리고 div.#root 요소에 웹페이지의 내용이 비어있는 것을 볼 수 있음. 요청된 리소스(html, json 등)가 응답되면 클라이언트에서 div#root 요소에 응답받은 데이터를 기반으로 html을 생성해 추가함

이를 통해 불필요한 리소스의 중복 요청을 방지할 수 있음. 또한 페이지 전체를 리렌더링할 필요없이 갱신이 필요한 일부만 갱신하면 되므로 빠른 퍼포먼스와 부드러운 화면 표시 효과를 기대할 수 있어 새로고침이 없는 보다 향상된 사용자 경험을 구현할 수 있다는 장점이 있음

ajax 요청은 주소창의 URL을 변경시키지 않음. 이는 브라우저의 뒤로가기, 앞으로가기 등의 **history 관리가 동작하지 않음을 의미함**. 따라서 history.back(), history.go(n) 등도 동작하지 않음. 주소창의 URL이 변경되지 않기 때문에 새로고침을 해도 언제나 첫페이지가 다시 로딩됨. 동일한 하나의 URL로 동작하는 ajax 방식은 SEO 이슈에서도 자유로울 수 없음

### 3.3 Hash 방식

ajax 방식은 불필요한 리소스 중복 요청을 방지할 수 있고 새로고침이 없는 사용자 경험을 구현할 수 있다는 방점이 있지만 history 관리가 되지 않는 단점이 있음. 이를 보완한 방법이 Hash 방식임

Hash 방식은 URL의 fragment identifier(ex: #about)의 고유 기능인 앵커를 사용함. fragment identifier는 hash mark 또는 hash라고 부르기도 함

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>SPA-Router - Hash</title>
    <link rel="stylesheet" href="css/style.css" />
    <script type="module" src="js/index.js"></script>
  </head>
  <body>
    <nav>
      <ul>
        <li><a href="#">Home</a></li>
        <li><a href="#service">Service</a></li>
        <li><a href="#about">About</a></li>
      </ul>
    </nav>
    <div id="root">Loading...</div>
  </body>
</html>
```

위 예제를 살펴보면 link tag의 href 어트리뷰트에 hash를 사용하고 있음. 즉, 내비게이션이 클릭되면 hash가 추가된 URI가 주소창에 표시됨. 단, **URL이 동일한 상태에서 hash만 변경되면 브라우저는 서버에 어떠한 요청도 하지 않음**. 즉, URL의 hash는 변경되어도 서버에 새로운 요청을 보내지 않으며 따라서 페이지가 갱신되지 않음. hash는 요청을 위한 것이 아니라 fragment identifier의 고유 기능인 앵커로 웹페이지 내부에서 이동을 위한 것이기 때문임

또한 hash 방식은 서버에 새로운 요청을 보내지 않으며 따라서 페이지가 갱신되지 않지만 페이지마다 **고유의 논리적 URL이 존재하므로 history 관리에 아무런 문제가 없음**

자바스크립트로 구현하면 아래와 같음

```
// index.js
// components.js는 위와 동일
import { Home, Service, About, NotFound } from './components.js';

const $root = document.getElementById('root');

const routes = [
  { path: '', component: Home },
  { path: 'service', component: Service },
  { path: 'about', component: About },
];

const render = async () => {
  try {
    // url의 hash를 취득
    const hash = window.location.hash.replace('#', '');
    const component = routes.find(route => route.path === hash)?.component || NotFound;
    $root.replaceChildren(await component());
  } catch (err) {
    console.error(err);
  }
};

// 네비게이션을 클릭하면 url의 hash가 변경되기 때문에 history 관리가 가능
// 단, url의 hash만 변경되면 서버로 요청은 수행하지 않음
// url의 hash가 변경하면 발생하는 이벤트인 hashchange 이벤트를 사용하여 hash의 변경을 감지하여 필요한 ajax 요청을 수행
// hash 방식의 단점은 url에 /#foo와 같은 해시뱅(HashBang)이 들어감
window.addEventListener('hashchange', render);

// 새로고침을 하면 DOMContentLoaded 이벤트가 발생하고
// render 함수는 url의 hash를 취득해 새로고침 직전에 렌더링되었던 페이지를 다시 렌더링
window.addEventListener('DOMContentLoaded', render);
```

hash 방식은 url의 hash가 변경하면 발생하는 이벤트인 hashchange 이벤트를 사용해 hash의 변경을 감지하고 url의 hash를 취득해 필요한 ajax 요청을 수행함

hash 방식의 단점은 url에 불필요한 #이 들어간다는 것임. 일반적으로 hash 방식을 사용할 때 #!을 사용하기도 하는데 이를 해시뱅(Hash-bang)이라고 부름

hash 방식은 과도기적 기술임. HTML5의 History API인 pushState가 모든 브라우저에서 지원이 된다면 해시뱅은 사용하지 않아도 되지만 현재 pushState는 일부의 브라우저(IE 10 이상)에서만 지원이 가능함

또 다른 문제는 SEO 이슈임. 웹 크롤러는 검색 엔진이 웹사이트의 콘텐츠를 수집하기 위해 HTTP와 URL 스펙(RFC-2396같은)을 따름. 이러한 크롤러는 JavaScript를 실행시키지 않기 때문에 hash 방식으로 만들어진 사이트의 콘텐츠를 수집할 수 없음. 구글은 해시뱅을 일반적인 URL로 변경시켜 이 문제를 해결한 것으로 알려져 있지만 다른 검색 엔진은 hash 방식으로 만들어진 사이트의 콘텐츠를 수집할 수 없을 수도 있음

### 3.4 pjax 방식

hash 방식의 가장 큰 단점은 SEO 이슈임. 이를 보완한 방법이 HTML5의 History API인 pushState와 popstate 이벤트를 사용한 pjax(pushState + ajax) 방식임. pushState와 popstate은 IE 10 이상에서 동작함

link tag의 href 어트리뷰트에 path를 사용하고 있음. 따라서 내비게이션이 클릭되면 path가 추가된 URL이 서버로 요청됨. 하지만 pjax 방식은 내비게이션 클릭 이벤트를 캐치하고 preventDefault 메서드를 사용해 서버로의 요청을 방지함. 이후, href 어트리뷰트에 path을 사용하여 ajax 요청을 하는 방식임

이때 ajax 요청은 브라우저 주소창의 URL을 변경시키지 않아 history 관리가 불가능함. 이때 사용하는 것이 pushState 메서드임. **pushState 메서드는 주소창의 URL을 변경하고 URL을 history entry로 추가하지만 서버로 HTTP 요청을 하지는 않음**

pjax 방식에서 사용하는 history.pushState 메서드는 주소창의 url을 변경하지만 HTTP 요청을 서버로 전송하지는 않음. 따라서 페이지가 갱신되지 않음. 하지만 페이지마다 고유의 URL이 존재하므로 history 관리에 아무런 문제가 없음. 또한 hash를 사용하지 않으므로 SEO에도 문제가 없음

자바스크립트로 구현하면 다음과 같음

```
// index.js
// components.js는 위와 동일
import { Home, Service, About, NotFound } from './components.js';

const $root = document.getElementById('root');
const $navigation = document.getElementById('navigation');

const routes = [
  { path: '/', component: Home },
  { path: '/service', component: Service },
  { path: '/about', component: About },
];

// TODO: path를 상태로 관리
const render = async path => {
  // $navigation의 a 요소를 클릭하면 path(a 요소의 href)가 전달됨
  // 하지만 웹페이지가 처음 로딩되거나 앞으로/뒤로 가기 버튼을 클릭하면 path를 전달하지 않음
  // 이때 window.location.pathname를 키로 routes에서 컴포넌트를 결정해 뷰를 전환함
  const _path = path ?? window.location.pathname;

  try {
    const component = routes.find(route => route.path === _path)?.component || NotFound;
    $root.replaceChildren(await component());
  } catch (err) {
    console.error(err);
  }
};

$navigation.addEventListener('click', e => {
  if (!e.target.matches('#navigation > li > a')) return;

  /**
   * 네비게이션을 클릭하면 주소창의 url이 변경되므로 HTTP 요청이 서버로 전송됨
   * preventDefault를 사용하여 이를 방지하고 history 관리를 위한 처리를 실행함
   */
  e.preventDefault();

  // 이동할 페이지의 path
  const path = e.target.getAttribute('href');

  // 현재 페이지와 이동할 페이지가 같으면 이동하지 않음
  if (window.location.pathname === path) return;

  // pushState는 주소창의 url을 변경하지만 HTTP 요청을 서버로 전송하지는 않음
  window.history.pushState(null, null, path);
  render(path);
});

/**
 * pjax 방식은 hash를 사용하지 않으므로 hashchange 이벤트를 사용할 수 없음
 * popstate 이벤트는 pushState에 의해 발생하지 않고 앞으로/뒤로 가기 버튼을 클릭하거나
 * history.forward/back/go(n)에 의해 history entry가 변경되면 발생함
 * 앞으로/뒤로 가기 버튼을 클릭하면 window.location.pathname를 참조해 뷰를 전환함
 */
window.addEventListener('popstate', () => {
  console.log('[popstate]', window.location.pathname);
  render();
});

/**
 * 웹페이지가 처음 로딩되면 window.location.pathname를 확인해 페이지를 이동시킴
 * 새로고침을 클릭하면 현 페이지(예를 들어 localhost:5004/service)가 서버에 요청됨
 * 이에 응답하는 처리는 서버에서 구현해야함
 */
window.addEventListener('DOMContentLoaded', () => {
  render();
});
```

단, 브라우저의 새로고침 버튼을 클릭하면 브라우저 주소창의 url이 변경되지 않는 ajax 방식과 해시(fragment identifier)만 추가되는 hash 방식은 서버에 별도의 요청을 보내지 않지만 pjax 방식은 브라우저 주소창의 url이 변경되기 때문에 요청(예를 들어 localhost:5004/service)이 서버로 전달됨. 즉, pjax 방식은 서버 렌더링 방식과 ajax 방식이 혼재되어 있는 방식으로 서버의 지원이 필요함

## 4. Conclusion

| 구분             | 히스토리 관리 | SEO 대응 | 사용자 경험 | 서버렌더링 | 구현 난이도 | IE 대응 |
| ---------------- | ------------- | -------- | ----------- | ---------- | ----------- | ------- |
| 전통적 링크 방식 | O             | O        | X           | O          | 간단        |         |
| ajax 방식        | X             | X        | O           | X          | 보통        | 7이상   |
| hash 방식        | O             | X        | O           | X          | 보통        | 8이상   |
| pjax 방식        | O             | O        | O           | △          | 복잡        | 10이상  |

어플리케이션 상황을 고려해 적절한 방법을 선택하자
