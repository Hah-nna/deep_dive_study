동기식 처리 모델은 직렬적으로 태스트를 수행함. 즉, 태스크는 순차적으로 실행되며 어떤 작업이 수행 중이면 다음 작업은 대기하게 됨

예를 들어 서버에서 어떤 데이터를 가져와서 화면에 표시하는 작업을 할 때, 서버에 데이터를 요청하고 데이터가 응답될 때까지 이후 태스크들은 블로킹됨

<div>
<img src="https://poiemaweb.com/img/block_nonblock.png" width="50%" height="50%">

<img src="https://poiemaweb.com/img/synchronous.png" width="50%" height="50%">
</div>

아래의 예시는 동기식으로 작동하는 코드임. 순차적으로 실행됨

```
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  console.log('func2');
  func3();
}

function func3() {
  console.log('func3');
}

func1();
```

비동기식 처리 모델은 병렬적으로 태스크를 수행함. 즉, 태스크가 종료되지 않은 상태라 하더라도 대기하지 않고 다음 태스크를 실행함. 예를 들어 서버에서 데이터를 가져와서 화면에 표시하는 태스크를 수행할 때, 서버에 데이터를 요청한 이후 서버로부터 데이터가 응답될 때까지 대기하지 않고 즉시 다음 태스크를 수행함. 이후 서버로부터 데이터가 응답되면 이벤트가 발생하고 이벤트 핸들러가 데이터를 가지고 수행할 태스크를 계속해 수행함
자바스크립트의 대부분의 DOM 이벤트 핸들러와 Timer 함수, Ajax 요청은 비동기식 처리 모델로 동작함

<div>
<img src="https://poiemaweb.com/img/asynchronous.png" width="50%" height="50%">
</div>

아래는 비동기로 동작하는 코드임. 순차적으로 실행되지 않음

```
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  setTimeout(function() {
    console.log('func2');
  }, 0);

  func3();
}

function func3() {
  console.log('func3');
}

func1();
// func1
// func3
// func2
```

위 예제를 실행하면 setTimeout메소드에 두 번쨰 인수 인터벌을 0초로 설정하여도 콘솔에 'func1 func2 func3'의 순서로 로그가 출력되지 않음. 이는 setTimeout 메소드가 비동기 함수이기 때문임

<div>
<img src="https://poiemaweb.com/img/settimeout.png" width="50%" height="50%">
</div>

함수 func1이 호출되면 함수 func1은 call stack에 쌓임. 그리고 함수 func1은 함수 func2를 호출하므로 함수 func2가 call stack에 쌓이고 setTimeout가 호출됨. **setTimeout의 콜백함수는 즉시 실행되지 않고 지정 대기 시간만큼 기다리다가 'tick' 이벤트가 발생하면 대스크 큐로 이동한 후 call stack이 비어졌을 때 call stack으로 이동되어 실행됨**

<div>
<img src="https://poiemaweb.com/img/event-loop.gif" width="50%" height="50%">
</div>
