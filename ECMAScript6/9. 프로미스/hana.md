## 1. 프로미스란?

자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용함. 하지만 전통적인 콜백 패턴은 콜백 헬로 인해 가독성이 나쁘고 비동기 처리 중 발생한 에러의 처리가 곤란하며 여러 개의 비동기 처리를 한번에 처리하는 데도 한계가 있음
ES6에서는 비동기 처리를 위한 또 다른 패턴으로 프로미스를 도입함. 프로미스는 전통적인 콜백 패턴이 가진 단점을 보완하며 비동기 처리 시점을 명확하게 표현할 수 있다는 장점이 있음

## 2. 콜백 패턴의 단점

### 2.1 콜백 헬

동기식 처리모델은 직렬적으로 태스크를 수행함. 즉, 태스크는 순차적으로 실행되며 어떤 작업이 수행중이면 다름 태스크는 대기하게 됨. 예를 들어 서버에서 데이터를 가져와서 화면에 표시하는 태스크를 수행할 때 서버에 데이터를 요청하고 데이터가 응답될 때까지 태스크들은 블로킹됨

비동기 처리모델은 병렬적으로 태스크를 수행함. 즉, 태스크가 종료되지 않은 상태라 하더라도 대기하지 않고 즉시 다음 태스크를 실행함. 예를 들어 서버에서 데이터를 가져와서 화면에 표시하는 태스크를 수행할 때, 서버에서 데이터를 요청한 이후 서버로부터 데이터가 응답될 때까지 대기하지 않고 즉시 다음 태스크를 수행함. 이후 서버로부터 데이터가 응답되면 이벤트가 발생하고 이벤트 핸들러가 데이터를 가지고 수행할 태스크를 계속해 수행함. 자바스크립트의 대부분의 DOM이벤트와 Timer 함수(setTimeout, setInterval), Ajax 요청은 비동기식 처리 모델로 동작함

자바스크립트에서 빈번하게 사용되는 비동기식 처리 모델은 요청을 병렬로 처리하여 다른 요청이 블로킹되지 않는 장점이 잇음

하지만 비동기 처리를 위해 콜백 패턴을 사용하면 처리 순서를 보장하기 위해 여러 개의 콜백함수가 네스팅(nesting, 중첩)되어 복잡도가 높아지는 콜백헬이 발생하는 단점이 있음. 콜백 헬은 가독성을 나쁘게 하며 실수를 유발하는 원인이 됨. 아래는 콜백 헬이 발생하는 전형적인 사례임

```
step1(function(value1) {
  step2(value1, function(value2) {
    step3(value2, function(value3) {
      step4(value3, function(value4) {
        step5(value4, function(value5) {
            // value5를 사용하는 처리
        });
      });
    });
  });
});
```

콜백 헬이 발생하는 이유는 다음과 같음
비동기 처리 모델은 실행완료를 기다리지 않고 즉시 그 다음 태스크를 실행함. 따라서 비동기 함수 내에서 처리 결과를 반환 (또는 전역 변수에의 할당)하면 기대한 대로 동작하지 않음.

```
<!DOCTYPE html>
<html>
<body>
  <script>
    // 비동기 함수
    function get(url) {
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // 서버 응답 시 호출될 이벤트 핸들러
      xhr.onreadystatechange = function () {
        // 서버 응답 완료가 아니면 무시
        if (xhr.readyState !== XMLHttpRequest.DONE) return;

        if (xhr.status === 200) { // 정상 응답
          console.log(xhr.response);
          // 비동기 함수의 결과에 대한 처리는 반환할 수 없음
          return xhr.response; // 1
        } else { // 비정상 응답
          console.log('Error: ' + xhr.status);
        }
      };

      // 비동기 방식으로 Request 오픈
      xhr.open('GET', url);
      // Request 전송
      xhr.send();
    }

    // 비동기 함수 내의 readystatechange 이벤트 핸들러에서 처리 결과를 반환(1)하면 순서가 보장되지 않음
    const res = get('http://jsonplaceholder.typicode.com/posts/1');
    console.log(res); // 2 undefined
  </script>
</body>
</html>
```

비동기 함수 내의 readystatechange 이벤트 핸들러에서 처리 결과를 반환(1)하면 순서가 보장되지 않음. 즉, 2에서 get 함수가 반환한 값을 참조할 수 없음. 그 이유에 대해 살펴보자

get 함수가 호출되면 get 함수의 실행 컨텍스트가 생성되고 호출 스택(실행 컨텍스트 스택)에서 실행됨. get 함수가 반환하는 xhr.response는 readystatechange 이벤트 핸들러가 반환함. readystatechange 이벤트는 발생하는 시점을 명확히 알 수 없지만 반드시 get 함수가 종료한 이후 발생함. get 함수의 마지막 문인 xhr.send()가 실행되어야 request를 전송하고 request를 전송해야 readystatechange 이벤트가 발생할 수 있기 때문임

get 함수가 종료하면 곧바로 console.log(2)가 호출되어 호출 스택에 들어가 실행됨. console.log가 호출되기 직전에 readystatechange 이벤트가 이미 발생했다하더라도 이벤트 핸들러는 console.log보다 먼저 실행되지 않음

readystatechange 이벤트의 이벤트 핸들러는 이벤트가 발생하면 즉시 실행되는 것이 아님. 이벤트가 발생하면 일단 태스크 큐로 들어가고 호출 스택이 비면 그때 이벤트 루프에 의해 호출 스택으로 들어가 실행됨. console.log 호출 시점 이전에 readystatechange 이벤트가 이미 발생했다하더라도 get 함수가 종료하면 곧바로 console.log가 호출되어 호출 스택에 들어가기 때문에 readystatechange 이벤트의 이벤트 핸들러는 console.log가 종료되어 호출 스택에서 빠진 이후 실행됨. 만약 get 함수 이후에 console.log가 100번 호출된다면 readystatechange 이벤트의 이벤트 핸들러는 모든 console.log가 종료한 이후에나 실행됨

때문에 get 함수의 반환 결과를 가지고 후속 처리를 할 수 없음. 즉, 비동기 함수의 처리 결과를 반환하는 경우 순서가 보장되지 않기 때문에 그 반환 결과를 가지고 후속 처리를 할 수 없음. 즉, 비동기 함수의 처리 결과에 대한 처리는 비동기 함수의 콜백 함수 내에서 처리해야 함. 이로 인해 콜백 헬이 발생함

만일 비동기 함수의 처리 결과를 가지고 다른 비동기 함수를 호출해야 하는 경우, 함수의 호출이 중첩(nesting)이 되어 복잡도가 높아지는 현상이 발생하는데 이를 **Callback Hell**이라 함

Callback Hell은 코드의 가독성을 나쁘게 하고 복잡도를 증가시켜 실수를 유발하는 원인이 되며 에러 처리가 곤란함

### 2.1 에러 처리의 한계

콜백 방식의 비동기 처리가 갖는 문제점 중에 가장 심각한 것은 에러 처리가 곤란하다는 것임.

```
try {
  setTimeout(() => { throw new Error('Error!'); }, 1000);
} catch (e) {
  console.log('에러 캐치 못 함ㅎ');
  console.log(e);
}
```

try 블록 내에서 setTimeout 함수가 실행되면 1초 후에 콜백 함수가 실행되고 이 콜백 함수는 예외를 발생시킴. 하지만 이 예외는 catch 블록에서 캐치되지 않음. 그 이유는 다음과 같음

비동기 처리 함수의 콜백 함수는 해당 이벤트(timer 함수의 tick 이벤트, XMLHttpRequest의 readystatechange 이벤트 등)가 발생하면 태스트 큐(Task queue)로 이동한 후 호출 스택이 비어졌을 때, 호출 스택으로 이동되어 실행됨. setTimeout 함수는 비동기 함수이므로 콜백 함수가 실행될 때까지 기다리지 않고 즉시 종료되어 호출 스택에서 제거됨. 이후 tick 이벤트가 발생하면 setTimeout 함수의 콜백 함수는 태스트 큐로 이동한 후 호출 스택이 비어졌을 때 호출 스택으로 이동되어 실행된다. 이때 setTimeout 함수는 이미 호출 스택에서 제거된 상태임. 이것은 setTimeout 함수의 콜백 함수를 호출한 것은 setTimeout 함수가 아니다라는 것을 의미함. setTimeout 함수의 콜백 함수의 호출자(caller)가 setTimeout 함수라면 호출 스택에 setTimeout 함수가 존재해야 하기 때문임

예외(exception)는 호출자(caller) 방향으로 전파됨. 하지만 위에서 살펴본 바와 같이 setTimeout 함수의 콜백 함수를 호출한 것은 setTimeout 함수가 아님. 따라서 setTimeout 함수의 콜백 함수 내에서 발생시킨 에러는 catch 블록에서 캐치되지 않아 프로세스는 종료됨

이러한 문제를 극복하기 위해 Promise가 제안되었음. Promise는 ES6에 정식 채택되어 IE를 제외한 대부분의 브라우저가 지원하고 있음

## 3. 프로미스의 생성

## 4. 프로미스의 후속 처리 메소드

## 5. 프로미스의 에러 처리

## 6. 프로미스 체이닝

## 7. 프로미스의 정적 메소드

### 7.1. Promise.resolve/Promise.reject

### 7.2. Promise.all

### 7.2. Promise.race
