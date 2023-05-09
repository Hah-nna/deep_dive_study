## 1. introduction

이벤트는 어떤 사건을 의미함. 브라우저에서 이벤트란 예를 들어 사용자가 버튼을 클릭했을 때, 웹페이지가 로드되었을 때와 같은 것인데 이것은 DOM 요소와 관련이 있음

이벤트가 발생하는 시점이나 순서를 사전에 인지할 수 없으므로 일반적인 제어 흐름과는 다른 접근 방식이 필요함. 즉, 이벤트가 발생하면 누군가 이를 감지할 수 있어야 하며 그에 대응하는 처리를 호출해주어야 함.

브라우저는 이벤트를 감지할 수 있으며 이벤트 발생시에는 통지해줌. 이 과정을 통해 사용자와 웹페이지는 상호작용이 가능하게 됨

이벤트가 발생하면 그에 맞는 반응을 해야함. 이를 위해 이벤트는 일반적으로 함수에 연결되며 그 함수는 이벤트가 발생하기 전에는 실행되지 않다가 이벤트가 발생되면 실행됨. 이 함수를 이벤트 핸들러라 하며 이벤트에 대응하는 처리를 기술함

## 2. 이벤트 루프와 동시성(concurrency)

브라우저는 단일 스레드에서 이벤트 드리븐 방식으로 동작함
단일 스레드는 스레드가 하나뿐이라는 의미히며 이말은 곧 하나의 task만을 처리할 수 있다는 것을 의미함. 하지만 실제로 동작하는 웹 애플리케이션은 많은 task가 동시에 처리되는 것처럼 느껴짐. 이처럼 자바스크립트의 동시성을 지원하는 것이 바로 이벤트 루프임

브라우저 환경을 그림으로 표현하자면 아래와 같음

<div>
<img src="https://poiemaweb.com/img/event-loop.png" width="50%" height="50">
</div>

대부분의 자바스크립트 엔진은 크게 2개의 영역으로 나뉨

**call stack**

작업이 요청되면(함수가 호출되면) 요청된 작업은 call stack에 쌓이게 되고 순차적으로 실행됨. 자바스크립트는 단 하나의 call stack을 사용하기 때문에 해당 task가 종료되기 전까지는 다른 어떤 task도 수행될 수 없음

**Heap**
동적으로 생성된 객체 인스턴스가 할당되는 영역임

이와 같이 자바스크립트 엔진은 단순히 작업이 요청되면 call stack을 사용해 요청된 작업을 순차적으로 실행할 뿐임. 앞에서 언급한 동시성을 지원하기 위해 필요한 비동기요청 처리는 자바스크립트 엔진을 구동하는 환경 즉, 브라우저(또는 Node.js)가 담당함

**Event Queue**
비동기 처리함수의 콜백함수, 비동기식 이벤트 핸들러, Timer 함수의 콜백함수가 보관되는 영역으로 이벤트 루프에 의해 특정 시점(call stack이 비어졌을 때)에서 순차적으로 call stack으로 이동되어 실행됨

**Event Loop**
call stack 내에서 현재 실행중인 task가 있는지 그리고 event queue에 task가 있는지 반복해 확인함. 만약 call stack이 비어있다면 event queue 내의 task가 call stack으로 이동하고 실행됨

```
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  setTimeout(function () {
    console.log('func2');
  }, 0);

  func3();
}

function func3() {
  console.log('func3');
}

func1();
```

함수 func1이 호출되면 함수 func1은 call stack에 쌓임. 그리고 함수 func1은 함수 func2을 호출하므로 함수 func2가 call stack에 쌓이고 setTimeout가 호출됨
**setTimeout의 콜백함수는 즉시 실행되지 않고 지정 대기 시간만큼 기다리다가 'tick' 이벤트가 발생하면 태스크 큐로 이동한 후 Call Stack이 비어졌을 때 Call Stack으로 이동되어 실행됨**

<div>
<img src="https://poiemaweb.com/img/event-loop.gif" width="50%" height="50%">
</div>

DOM 이벤트 핸들러도 이와 같이 동작함

```
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  // <button class="foo">foo</button>
  const elem = document.querySelector('.foo');

  elem.addEventListener('click', function () {
    this.style.backgroundColor = 'indigo';
    console.log('func2');
  });

  func3();
}

function func3() {
  console.log('func3');
}

func1();
```

함수 func1이 호출되면 함수 func1은 call stack에 쌓임. 그리고 함수 func1은 함수 func2를 호출하므로 함수 func2가 call stack에 쌓이고 addEventListener가 호출됨.
addEventListener의 콜백함수는 foo 버튼이 클릭되어 click 이벤트가 발생하면 태스크 큐로 이동한 후 call stack이 비어졌을 때 call stack으로 이동되어 실행됨

## 3. 이벤트의 종류

대표적인 이벤트만 정리함. 자세한 사항은 [여기]("https://developer.mozilla.org/en-US/docs/Web/Events") 를 참조하면 됨

### 3.1 UI event

| event  | description                                                              |
| ------ | ------------------------------------------------------------------------ |
| load   | 웹페이지의 로드가 완료 되었을 때                                         |
| unload | 웹페이지가 언로드 될 때(주로 새로운 페이지 요청한 경우)                  |
| error  | 브라우저가 자바스크립트 오류를 만났거나 요청한 자원이 존재하지 않는 경우 |
| resize | 브라우저 창의 크리를 조절했을 때                                         |
| scroll | 사용자가 페이지를 위 아래로 스크롤할 때                                  |
| select | 텍스트를 선택했을 때                                                     |

### 3.2 keyboard event

| event    | description            |
| -------- | ---------------------- |
| keydown  | 키를 누르고 있을 때    |
| keyup    | 누르고 있던 키를 뗄 때 |
| keypress | 키를 누르고 떼었을 때  |

keyup과 keypress의 차이점은?

**keyup**

- 사용자가 키를 누른 후 떼는 순간 발생함
- 이벤트가 발생한 순간 떼어낸 키의 값이(event.keyCode나 event.which 프로퍼티를 통해 전달됨)
- 모든 키를 처리할 수 있음 (특수키나 조합키 인식O)

=> 결론 : `keypress`는 문자를 입력할 때 사용, `keyup`은 모든 키 입력을 처리할 때 사용

**keypress**

- 사용자가 키를 누르는 동안 발생함
- 이벤트가 발생한 순간 이미 문자가 입력되었음(event.keyCode나 event.charCode 프로퍼티를 통해 입력된 문자 바로 확인 가능)
- 문자키를 눌렀을 때만 발생함(특수키나 조합키 인식x)

### 3.3 mouse event

| event     | description                                             |
| --------- | ------------------------------------------------------- |
| click     | 마우스 버튼을 클릭했을 때                               |
| dbclick   | 마우스 버튼을 더블 클릭했을 때                          |
| mousedown | 마우스 버튼을 누르고 있을 때                            |
| mouseup   | 누르고 있던 마우스 버튼을 뗄 떼                         |
| mousemove | 마우스를 움직일 때(터치 스크린에서 동작x)               |
| mouseover | 마우스를 요소 위로 움직였을 때(터치 스크린에서 동작x)   |
| mouseout  | 마우스를 요소 밖으로 움직였을 때(터치 스크린에서 동작x) |

### 3.4 focus event

| event         | description               |
| ------------- | ------------------------- |
| focus/focusin | 요소가 포커스를 얻었을 때 |
| blur/focusout | 요소가 포커스를 잃었을 때 |

### 3.5 form event

| event  | description                                                 |
| ------ | ----------------------------------------------------------- |
| input  | input 또는 textarea 요소의 값이 변경되었을 때               |
| ---    | contenteditable 어트리뷰트를 가진 요소의 값이 변경되었을 때 |
| change | select box, checkbox, radio button의 상태가 변경되었을 때   |
| submit | form을 submit할 때(버튼 또는 키)                            |
| reset  | reset 버튼을 클릭할 때(최근에는 사용안 함)                  |

### 3.6 clipboard event

| event | description             |
| ----- | ----------------------- |
| cut   | 콘텐츠를 잘라내기할 때  |
| copy  | 콘텐츠를 복사할 때      |
| paste | 콘텐츠를 붙여넣기 할 때 |

## 4. 이벤트 핸들러 등록

이벤트가 발생했을 때 동작할 이벤트 핸들러를 이벤트에 동작하는 방법은 다음의 3가지임

### 4.1 인라인 이벤트 핸들러 방식

HTML 요소의 이벤트 핸들러 어트리뷰트에 이벤트 핸들러를 등록하는 방법

```
<!DOCTYPE html>
<html>
<body>
  <button onclick="myHandler()">Click me</button>
  <script>
    function myHandler() {
      alert('Button clicked!');
    }
  </script>
</body>
</html>
```

이 방식은 더이상 사용되지 않으며 사용해서도 안됨. 오래된 코드에서 간혹 이 방식을 사용한 것이 있디 때문에 알아둘 필요는 있음.

주의할 것은 onclick과 같이 on으로 시작하는 이벤트 어트리뷰트의 값으로 함수 호출을 전달한다는 것임. 다음에 살펴볼 이벤트 핸들러 프로퍼티 방식에는 DOM 요소의 이벤트 핸들러 프로퍼티에 함수 호출이 아닌 함수를 전달함

이때 이벤트 어트리뷰트의 값으로 전달한 함수 호출이 즉시 호출되는 것은 아님. 사실은 이벤트 어트리뷰트 키를 이름으로 갖는 함수를 암묵적으로 정의하고 그 함수의 몸체에 이벤트 어트리뷰트의 값으로 전달한 함수 호출을 문으로 가짐. 위 예제의 경우, button 요소의 onclick 프로퍼티에 함수 function onclick(event) { foo(); }가 할당됨

즉, 이벤트 어트리뷰트의 값은 암묵적으로 정의되는 이벤트 핸들러의 문임. 따라서 아래와 같이 여러 개의 문을 전달할 수 있음

```
<!DOCTYPE html>
<html>
<body>
  <button onclick="myHandler1(); myHandler2();">Click me</button>
  <script>
    function myHandler1() {
      alert('myHandler1');
    }
    function myHandler2() {
      alert('myHandler2');
    }
  </script>
</body>
</html>
```

### 4.2 이벤트 핸들러 프로퍼티 방식

인라인 이벤트 핸들러 방식처럼 HTML과 자바스크립트가 뒤섞이는 문제는 해결할 수 있는 방식임. 하지만 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만을 바인딩할 수 있다는 단점이 있음

```
<!DOCTYPE html>
<html>
<body>
  <button class="btn">Click me</button>
  <script>
    const btn = document.querySelector('.btn');

    // 이벤트 핸들러 프로퍼티 방식은 이벤트에 하나의 이벤트 핸들러만을 바인딩할 수 있음
    // 첫번째 바인딩된 이벤트 핸들러 => 실행되지 않음
    btn.onclick = function () {
      alert('① Button clicked 1');
    };

    // 두번째 바인딩된 이벤트 핸들러
    btn.onclick = function () {
      alert('① Button clicked 2');
    };

    // addEventListener 메소드 방식
    // 첫번째 바인딩된 이벤트 핸들러
    btn.addEventListener('click', function () {
      alert('② Button clicked 1');
    });

    // 두번째 바인딩된 이벤트 핸들러
    btn.addEventListener('click', function () {
      alert('② Button clicked 2');
    });
  </script>
</body>
</html>
```

① Button clicked 1 -> ② Button clicked 1' -> ② Button clicked 2

첫 번째 바인딩된 이벤트 핸들러가 실행되지 않는 것은 두 번째 이벤트 핸들러로 덮어씌워졌기 때문임.
onlick이벤트는 해당 이벤트에 하나의 이벤트 만을 바인딩할 수 있음. 따라서 두 번째 바인딩된 이벤트 핸들러로 덮어씌워지면서 첫 번째 바인딩된 이벤트는 사라지게 됨
addEventListener 메소드는 하나의 이벤트에 여러 개의 이벤트 핸들러를 등록할 수 있기 때문에 두 개의 이벤트 핸들러가 모두 실행됨

### 4.3 addEventListener 메소드 방식

`addEventListener` 메소드를 이용해 해당 DOM 요소에 이벤트를 바인딩하고 해당 이벤트가 발생했을 때 실행될 콜백함수(이벤트 핸들러)를 지정함

<div>
<img src="https://poiemaweb.com/img/event_listener.png" width="50%" height="50%">
</div>

addEventListener 함수 방식은 이전 방식에 비해 아래와 같이 보다 나은 장점을 가짐

- 하나의 이벤트에 하나 이상의 이벤트 핸들러를 추가할 수 있음
- 캡처링과 버블링을 지원함
- HTML 요소뿐만 아니라 모든 DOM요소에 대해 동작함. 브라우저는 웹문서를 로드한 후 파싱하여 DOM을 생성함

addEventListener 메소드는 IE9 이상에서 동작함. IE8이하에서는 attachEvent를 사용함

```
if (elem.addEventListener) {    // IE 9 ~
  elem.addEventListener('click', func);
} else if (elem.attachEvent) {  // ~ IE 8
  elem.attachEvent('onclick', func);
}
```

아래는 addEventListener 메소드의 사용예시임

```
<!DOCTYPE html>
<html>
<body>
  <script>
    addEventListener('click', function () {
      alert('Clicked!');
    });
  </script>
</body>
</html>
```

위와 같이 대상 DOM요소(target)을 지정하지 않으면 window, 즉 DOM 문서를 포함한 브라우저의 윈도우에서 발생하는 click 이벤트에 이벤트 핸들러를 바인딩함. 따라서 브라우저 윈도우 어디를 클릭해도 이벤트 핸들러가 동작함

```
<!DOCTYPE html>
<html>
<body>
  <label>User name <input type='text'></label>

  <script>
    const input = document.querySelector('input[type=text]');

    input.addEventListener('blur', function () {
      alert('blur event occurred!');
    });
  </script>
</body>
</html>
```

위 예제는 input 요소에서 발생하는 blur 이벤트에 이벤트 핸들러를 바인딩함. 사용자 이름이 최소 2글자 이상이어야 한다는 규칙을 세우고 이에 부합하는지 확인해보자

```
<!DOCTYPE html>
<html>
<body>
  <label>User name <input type='text'></label>
  <em class="message"></em>

  <script>
    const input = document.querySelector('input[type=text]');
    const msg = document.querySelector('.message');

    input.addEventListener('blur', function () {
      if (input.value.length < 2) {
        msg.innerHTML = '이름은 2자 이상 입력해 주세요';
      } else {
        msg.innerHTML = '';
      }
    });
  </script>
</body>
</html>
```

2자 이상이라는 규칙이 바뀌면 이 규칙을 확인하는 모든 코드를 수정해야 함. 따라서 이러한 방식의 코딩은 바람직하지 않음. 이유는 규모가 큰 프로그램의 경우 수정과 테스트에 소요되는 자원의 낭비도 문제이지만 수정에는 거의 대부분 실수가 동반되기 때문임

2자 이상이라는 규칙을 상수화하고 함수의 인수로 전달도록 수정하자. 이렇게 하면 규칙이 변경되어도 함수는 수정하지 않아도 됨

그런데 addEventListener 메소드의 두번째 매개변수는 이벤트가 발생했을 때 호출될 이벤트 핸들러임. 이때 두번째 매개변수에는 함수 호출이 아니라 함수 자체를 지정하여야 함

```
function foo() {
  alert('clicked!');
}
// elem.addEventListener('click', foo()); // 이벤트 발생 시까지 대기하지 않고 바로 실행됨
elem.addEventListener('click', foo);      // 이벤트 발생 시까지 대기함
```

따라서 이벤트 핸들러 프로퍼티 방식과 이벤트 핸들러 함수에 인수를 전달할 수 없는 문제가 발생함.
이를 우회하는 방식은 다음과 같음

```
<!DOCTYPE html>
<html>
<body>
  <label>User name <input type='text'></label>
  <em class="message"></em>

  <script>
    const MIN_USER_NAME_LENGTH = 2; // 이름 최소 길이

    const input = document.querySelector('input[type=text]');
    const msg = document.querySelector('.message');

    function checkUserNameLength(n) {
      if (input.value.length < n) {
        msg.innerHTML = '이름은 ' + n + '자 이상이어야 합니다';
      } else {
        msg.innerHTML = '';
      }
    }

    input.addEventListener('blur', function () {
      // 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다.
      checkUserNameLength(MIN_USER_NAME_LENGTH);
    });

    // 이벤트 핸들러 프로퍼티 방식도 동일한 방식으로 인수를 전달할 수 있다.
    // input.onblur = function () {
    //   // 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다.
    //   checkUserNameLength(MIN_USER_NAME_LENGTH);
    // };
  </script>
</body>
</html>
```

## 5. 이벤트 핸들러 함수 내부의 this

### 5.1 인라인 이벤트 핸들러 방식

인라인 이벤트 핸들러 방식의 경우, 이벤트 핸들러는 일반 함수로서 호출되므로 이벤트 핸들러 내부의 this는 전역객체 window를 가리킴

```
<!DOCTYPE html>
<html>
<body>
  <button onclick="foo()">Button</button>
  <script>
    function foo () {
      console.log(this); // window
    }
  </script>
</body>
</html>
```

### 5.2 이벤트 핸들러 프로퍼티 방식

이벤트 핸들러 프로퍼티 방식에서 이벤트 핸들러는 메소드이므로 이벤트 햄들러 내부에서의 this는 이벤트에 바인딩된 요소를 가리킴. 이것은 이벤트 객체의 currentTarget 프로퍼티와 같음

```
<!DOCTYPE html>
<html>
<body>
  <button class="btn">Button</button>
  <script>
    const btn = document.querySelector('.btn');

    btn.onclick = function (e) {
      console.log(this); // <button id="btn">Button</button>
      console.log(e.currentTarget); // <button id="btn">Button</button>
      console.log(this === e.currentTarget); // true
    };
  </script>
</body>
</html>
```

### 5.3 addEventListener 메소드 방식

addEventListener 메소드에서 지정한 이벤트 핸들러는 콜백 함수이지만 이벤트 핸들러 내부의 this는 이벤트 리스너에 바인딩된 요소(currentTarget)를 가리킴. 이것은 이벤트 객체의 currentTarget 프로퍼티와 같음

```
<!DOCTYPE html>
<html>
<body>
  <button class="btn">Button</button>
  <script>
    const btn = document.querySelector('.btn');

    btn.addEventListener('click', function (e) {
      console.log(this); // <button id="btn">Button</button>
      console.log(e.currentTarget); // <button id="btn">Button</button>
      console.log(this === e.currentTarget); // true
    });
  </script>
</body>
</html>
```

## 6. 이벤트의 흐름

## 7. Event 객체

### 7.1 Event Property

#### 7.1.1 Event.target

#### 7.1.2 Event.currentTarget

#### 7.1.3 Event.type

#### 7.1.4 Event.cancelable

#### 7.1.5 Event.eventPhase

## 8. 이벤트 위임

## 9. 기본 동작의 변경

### 9.1 Event.preventDefault()

### 9.2 Event.stopPropagation()

### 9.3 preventDefault & stopPropagation
