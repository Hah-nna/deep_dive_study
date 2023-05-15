## 1. Ajax(Asynchronous JavaScript and XML)

브라우저에서 웹페이지를 요청하거나 링크를 클릭하면 화면 갱신이 발생함. 이것은 브라우저와 서버와의 통신에 의한 것임

서버는 요청받은 페이지(HTML)을 반환하는데 이때 HTML에서 로드하는 CSS나 자바스크립트 파일들도 같이 반환됨. 클라이언트 요청에 따라 서버는 정적인 파일을 반환할 수도 있고 서버 사이드 프로그램이 만들어낸 파일이나 데이터를 반환할 수도 있음. 서버로부터 웹페이지가 반환되면 클라이언트(브라우저)는 이를 렌더링하여 화면에 표시함

Ajax(Asynchronous JavaScript and XML)는 자바스크립트를 이용해 비동기적으로 서버와 브라우저가 데이터를 교환할 수 있는 통신 방식을 의미함

서버로부터 웹페이지가 반환되면 화면 전체를 갱신해야하는데 페이지 일부만을 갱신하고도 동일한 효과를 볼 수 있도록 하는 것이 Ajax임. 페이지 전체를 로드하여 렌더링할 필요가 없고 갱신이 필요한 일부만 로드하여 갱신하면 되므로 빠른 퍼포먼스와 부드러운 화면 표시 효과를 기대할 수 있음

## 2. Json(JavaScript Object Notation)

클라이언트와 서버 간에는 데이터 교환이 필요함. JSON은 클라이언트와 서버 간 데이터 교환을 위한 규칙, 즉 데이터 포맷을 말함
JSON은 일반 텍스트 포맷보다 효과적인 데이터 구조화가 가능하며 XML 포맷보다 가볍고 사용하기 간편해 가독성도 좋음
자바스크립트의 객체 리터럴과 매후 흡사함. 하지만 **JSON은 순수한 텍스트로 구성된 규칙이 있는 데이터 구조**임

```
{
  "name": "Lee",
  "gender": "male",
  "age": 20,
  "alive": true
}
```

key는 반드시 큰따옴표로 둘러싸야함(작은 따롬표 사용 불가)

### 2.1 Json.stringify

JSON.stringify 메소드는 객체를 JSON 형식의 문자열로 변환함

```
const o = { name: 'Lee', gender: 'male', age: 20 };

// 객체 => JSON 형식의 문자열
const strObject = JSON.stringify(o);
console.log(typeof strObject, strObject);
// string {"name":"Lee","gender":"male","age":20}

// 객체 => JSON 형식의 문자열 + prettify
const strPrettyObject = JSON.stringify(o, null, 2);
console.log(typeof strPrettyObject, strPrettyObject);
// JSON.stringify()의 첫번째 아규먼트는 변환할 값, 두번째 인수는 옵션 객체, 세 번째 아규먼트는 들여쓰기 수준을 지정함(숫자가 높을 수록 들여쓰기 많아짐)
만약 두 번째 인수로 함수를 사용하면 객체의 특정 프로퍼티만 JSON문자열로 변환하거나 객체의 프로퍼티를 특정 형식으로 변환할 수 있음

/*
string {
  "name": "Lee",
  "gender": "male",
  "age": 20
}
*/

// replacer
// 값의 타입이 Number이면 필터링되어 반환되지 않음

function filter(key, value) {
  // undefined: 반환하지 않음
  return typeof value === 'number' ? undefined : value;
}

// 객체 => JSON 형식의 문자열 + replacer + prettify
const strFilteredObject = JSON.stringify(o, filter, 2);
console.log(typeof strFilteredObject, strFilteredObject);

/*
string {
  "name": "Lee",
  "gender": "male"
}
*/

const arr = [1, 5, 'false'];

// 배열 객체 => 문자열
const strArray = JSON.stringify(arr);
console.log(typeof strArray, strArray); // string [1,5,"false"]

// replacer
// 모든 값을 대문자로 변환된 문자열을 반환
function replaceToUpper(key, value) {
  return value.toString().toUpperCase();
}

// 배열 객체 => 문자열 + replacer
const strFilteredArray = JSON.stringify(arr, replaceToUpper);
console.log(typeof strFilteredArray, strFilteredArray); // string "1,5,FALSE"
```

### 2.2 Json.parse

JSON.parse 메소드는 JSON 데이터를 가진 문자열로 객체를 변환함

> 서버로부터 브라우저로 전송된 JSON 데이터는 문자열임. 이 문자열을 객체로서 사용하려면 객체화하여야 하는데 이를 역직렬화(Deserializing)이라 함. 역직렬화를 위해서 내장 객체 JSON의 static 메소드인 JSON.parse를 사용함

```
const o = { name: 'Lee', gender: 'male', age: 20 };

// 객체 => JSON 형식의 문자열
const strObject = JSON.stringify(o);
console.log(typeof strObject, strObject);
// string {"name":"Lee","gender":"male","age":20}

const arr = [1, 5, 'false'];

// 배열 객체 => 문자열
const strArray = JSON.stringify(arr);
console.log(typeof strArray, strArray); // string [1,5,"false"]

// JSON 형식의 문자열 => 객체
const obj = JSON.parse(strObject);
console.log(typeof obj, obj); // object { name: 'Lee', gender: 'male' }

// 문자열 => 배열 객체
const objArray = JSON.parse(strArray);
console.log(typeof objArray, objArray); // object [1, 5, "false"]
```

배열이 JSON 형식의 문자열로 변환되어 있는 경우 JSON.parse는 문자열을 배열 객체로 변환함. 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환함

```
const todos = [
  { id: 1, content: 'HTML', completed: true },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'JavaScript', completed: false }
];

// 배열 => JSON 형식의 문자열
const str = JSON.stringify(todos);
console.log(typeof str, str);

//string [{"id":1,"content":"HTML","completed":true},{"id":2,"content":"CSS","completed":true},{"id":3,"content":"JavaScript","completed":false}]



// JSON 형식의 문자열 => 배열
const parsed = JSON.parse(str);
console.log(typeof parsed, parsed);
// object
(3) [{…}, {…}, {…}]
0: {id: 1, content: 'HTML', completed: true}
1: {id: 2, content: 'CSS', completed: true}
2: {id: 3, content: 'JavaScript', completed: false}
length: 3
[[Prototype]]: Array(0)

```

## 3. XMLHttpRequest

브라우저는 XMLHttpRequest객체를 이용해 Ajax 요청을 생성하고 전송함. 서버가 브라우저의 요청에 대해 응답을 반환하면 같은 XMLHttpRequest객체가 그 결과를 처리함

### 3.1 Ajax request

Ajax 요청 처리 예시

```
// XMLHttpRequest 객체의 생성
const xhr = new XMLHttpRequest();

// 비동기 방식으로 Request를 오픈
xhr.open('GET', '/users');

// Request를 전송
xhr.send();
```

#### 3.1.1 XMLHttpRequest.open

XMLHttpRequest 객체의 인스턴스를 생성하고 XMLHttpRequest.open 메소드를 사용하여 서버로싀 요청을 준비함. XMLHttpRequest.open의 사용법은 아래와 같음

```
XMLHttpRequest.open(method, url[, async])
```

| 매개변수 | 설명                                                             |
| -------- | ---------------------------------------------------------------- |
| method   | HTTP method(GET, POST, PUT, DELETE 등)                           |
| url      | 요청을 보낼 URL                                                  |
| async    | 비동기 조작 여부, 옵션으로 default는 true. 비동기방식으로 동작함 |

#### 3.1.2 XMLHttpRequest.send

XMLHttpRequest.send 메소드로 준비된 요청을 서버에 전달함
기본적으로 서버로 전송하는 데이터는 GET, POST, 메소드에 따라 그 전송 방식에 차이가 있음

- GET 메소드의 경우 URL의 일부분인 쿼리문자열로 데이터를 서버로 전송함
- POST 메소드의 경우 데이터(페이로드)를 Request body에 담아 전송함

<div align="center">
<img src="https://poiemaweb.com/img/HTTP_request+response_message.gif" width="50%" height="50%">
</div>

XMLHttpRequest.send 메소드에는 request body에 담아 전송할 인수를 전달할 수 있음

```
xhr.send(null);
// xhr.send('string');
// xhr.send(new Blob()); // 파일 업로드와 같이 바이너리 컨텐트를 보내는 방법
// xhr.send({ form: 'data' });
// xhr.send(document);
```

만약 요청 메소드가 GET인 경우 send 메소드의 인수는 무시되고 request body는 null로 설정됨

#### 3.1.3 XMLHttp.setRequestHeader

XMLHttpRequest.settHeader 메소드는 Http Request Header의 값을 설정함. setRequestHeader 메소드는 반드시 XMLHttpRequest.open 메소드 이후 호출됨

자주 사용하는 Request Header인 Content-type, Accept에 대해 살펴보자

**Content-type**
content-type은 request body에 담아 전송할 데이터의 MIME-type의 정보를 표현함. 자주 사용되는 MIME-type은 다음과 같음

| 타입                        | 서브타입                                           |
| --------------------------- | -------------------------------------------------- |
| text 타입                   | text/plain, text/html, text/css, text/javascript   |
| Application 타입            | application/json, application/x-www-form-urlencode |
| File을 업로드하기 위한 타입 | multipart/formed-data                              |

아래는 request body에 담아 서버로 전송할 데이터의 MIME-type을 지정하는 예시임

```
// json으로 전송하는 경우
xhr.open('POST', '/users');

// 클라이언트가 서버로 전송할 데이터의 MIME-type 지정: json
xhr.setRequestHeader('Content-type', 'application/json');

const data = { id: 3, title: 'JavaScript', author: 'Park', price: 5000};

xhr.send(JSON.stringify(data));
```

```
// x-www-form-urlencoded으로 전송하는 경우
xhr.open('POST', '/users');

// 클라이언트가 서버로 전송할 데이터의 MIME-type 지정: x-www-form-urlencoded
// application/x-www-form-urlencoded는 key=value&key=value...의 형태로 전송
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');

const data = { title: 'JavaScript', author: 'Park', price: 5000};

xhr.send(Object.keys(data).map(key => `${key}=${data[key]}`).join('&'));
// escaping untrusted data
// xhr.send(Object.keys(data).map(key => `${key}=${encodeURIComponent(data[key])}`).join('&'));
```

**Accept**

HTTP 클라이언트가 서버에 요청할 때 서버가 send back할 데이터의 MIME-type을 Accept로 지정할 수 있음

다음은 서버가 send back할 데이터의 MIME-type을 지정하는 예시임

```
// 서버가 센드백할 데이터의 MIME-type 지정: json
xhr.setRequestHeader('Accept', 'application/json');
```

만약 Accept 헤더를 설정하지 않으면 send 메소드가 호출될 때 Accept 헤더가 `*/*`로 전송됨

`*/*`로 설정되면 서버로부터 받을 수 있는 모든 미디어 타입 허용한다는 의미임

### 3.2 Ajax response

Ajax 응답 처리 예시

```
// XMLHttpRequest 객체의 생성
const xhr = new XMLHttpRequest();

// XMLHttpRequest.readyState 프로퍼티가 변경(이벤트 발생)될 때마다 onreadystatechange 이벤트 핸들러가 호출
xhr.onreadystatechange = function (e) {
  // readyStates는 XMLHttpRequest의 상태(state)를 반환
  // readyState: 4 => DONE(서버 응답 완료)
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status는 response 상태 코드를 반환 : 200 => 정상 응답
  if(xhr.status === 200) {
    console.log(xhr.responseText);
  } else {
    console.log('Error!');
  }
};
```

예시 코드를 살펴보면 XMLHttpRequest.send 메소드를 통해 서버에 Request를 전송하면 서버는 Response를 반환함. 하지만 언제 response가 클라이언트에 도달할 지는 알 수 없음. XMLHttpRequest.onreadystatechange는 response가 클라이언트에 도달하여 발생된 이벤트를 감지하고 콜백함수를 실행해줌. 이때 이벤트는 request에 어떤 변화가 발생한 경우 즉, XMLHttpRequest.readyState 프로퍼티가 변경된 경우 발생함

```
// XMLHttpRequest 객체의 생성
var xhr = new XMLHttpRequest();

// 비동기 방식으로 Request를 오픈
xhr.open('GET', 'data/test.json');

// Request를 전송한다
xhr.send();

// XMLHttpRequest.readyState 프로퍼티가 변경(이벤트 발생)될 때마다 콜백함수(이벤트 핸들러)를 호출함
xhr.onreadystatechange = function (e) {
  // 이 함수는 Response가 클라이언트에 도달하면 호출됨
};
```

XMLHttpRequest 객체는 response가 클라이언트에 도달했는지를 추적할 수 있는 프로퍼티를 제공함. 이 프로퍼티가 바로 XMLHttpRequest.readyState임. 만약 XMLHttpRequest.readyState의 값이 4인 경우, 정상적으로 response가 돌아온 경우임

| value | state            | description                                           |
| ----- | ---------------- | ----------------------------------------------------- |
| 0     | UNSENT           | XMLHttpRequest.open() 메소드 호출 이전                |
| 1     | OPENED           | XMLHttpRequest.open() 메소드 호출 완료                |
| 2     | HEADERS_RECEIVED | XMLHttpRequest.open() 메소드 호출 완료                |
| 3     | LOADING          | 서버 응답 중(XMLHttpRequest.responseText 미완성 상태) |
| 4     | DONE             | 서버 응답 완료                                        |

```
// XMLHttpRequest 객체의 생성
var xhr = new XMLHttpRequest();

// 비동기 방식으로 Request를 오픈
xhr.open('GET', 'data/test.json');

// Request를 전송
xhr.send();

// XMLHttpRequest.readyState 프로퍼티가 변경(이벤트 발생)될 때마다 콜백함수(이벤트 핸들러)를 호출
xhr.onreadystatechange = function (e) {
  // 이 함수는 Response가 클라이언트에 도달하면 호출됨

  // readyStates는 XMLHttpRequest의 상태(state)를 반환
  // readyState: 4 => DONE(서버 응답 완료)
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status는 response 상태 코드를 반환 : 200 => 정상 응답
  if(xhr.status === 200) {
    console.log(xhr.responseText);
  } else {
    console.log('Error!');
  }
};
```

XMLHttpRequest의 readyState가 4인 경우 서버 응답이 완료된 상태이므로 XMLHttpRequest.status가 200(정상 응답)임을 확인하고 정상인 경우 XMLHttpRequest.responseText를 취득함. XMLHttpRequest.responseText에는 서버가 전송한 데이터가 담겨있음
만약 서버 응답 완료 상태에만 반응하도록 하려면 readystatechange이벤트 대신 load 이벤트를 사용해도 됨. load 이벤트는 서버 응답이 완료된 경우에 발생함

```
// XMLHttpRequest 객체의 생성
var xhr = new XMLHttpRequest();

// 비동기 방식으로 Request를 오픈
xhr.open('GET', 'data/test.json');

// Request를 전송
xhr.send();

// load 이벤트는 서버 응답이 완료된 경우에 발생
xhr.onload = function (e) {
  // status는 response 상태 코드를 반환 : 200 => 정상 응답
  if(xhr.status === 200) {
    console.log(xhr.responseText);
  } else {
    console.log('Error!');
  }
};
```

readyState 속성과 onload 이벤트의 주요 차이점은 readyState 속성은 요청 상태를 나타내고, onload 이벤트는 요청이 완료될 때 발생한다는 것입니다

## 4. web server

웹서버는 브라우저와 같은 클라이언트로부터 HTTP 요청을 받아들이고 HTML문서와 같은 웹 페이지를 반환하는 컴퓨터 프로그램임

<div aling="center">
<img src="https://poiemaweb.com/img/cs.png" width="50%" height="50%">
</div>

## 5. Ajax 예제

### 5.1 Load HTML

Ajax응 사용해 웹페이지에 추가하기 가장 쉬운 데이터 형식은 HTML임. 별도의 작업없이 전송받은 데이터를 DOM에 추가하면 됨

```
<!-- 루트 폴더(webserver-express/public)/loadhtml.html -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="https://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id="content"></div>

    <script>
      // XMLHttpRequest 객체의 생성
      const xhr = new XMLHttpRequest();

      // 비동기 방식으로 Request를 오픈
      xhr.open('GET', 'data/data.html');

      // Request를 전송
      xhr.send();

      // Event Handler
      xhr.onreadystatechange = function () {
        // 서버 응답 완료 && 정상 응답
        if (xhr.readyState !== XMLHttpRequest.DONE) return;

        if (xhr.status === 200) {
          console.log(xhr.responseText);

          document.getElementById('content').innerHTML = xhr.responseText;
        } else {
          console.log(`[${xhr.status}] : ${xhr.statusText}`);
        }
      };
    </script>
  </body>
</html>
```

```
<!-- 루트 폴더(webserver-express/public)/data/data.html -->
<div id="tours">
  <h1>Guided Tours</h1>
  <ul>
    <li class="usa tour">
      <h2>New York, USA</h2>
      <span class="details">$1,899 for 7 nights</span>
      <button class="book">Book Now</button>
    </li>
    <li class="europe tour">
      <h2>Paris, France</h2>
      <span class="details">$2,299 for 7 nights</span>
      <button class="book">Book Now</button>
    </li>
    <li class="asia tour">
      <h2>Tokyo, Japan</h2>
      <span class="details">$3,799 for 7 nights</span>
      <button class="book">Book Now</button>
    </li>
  </ul>
</div>
```

### 5.2 Load JSON

서버로부터 브라우저로 전송된 JSON 데이터는 문자열임. 이 문자열을 객체화하여야하는데 이를 역직렬화(Deserializing)이라 함. 역직렬화를 위해서 내장 객체 JSON의 static 메소드인 JSON.parse()를 사용함

```
<!-- 루트 폴더(webserver-express/public)/loadjson.html -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="https://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id="content"></div>

    <script>
      // XMLHttpRequest 객체의 생성
      var xhr = new XMLHttpRequest();

      // 비동기 방식으로 Request를 오픈
      xhr.open('GET', 'data/data.json');

      // Request를 전송
      xhr.send();

      xhr.onreadystatechange = function () {
        // 서버 응답 완료 && 정상 응답
        if (xhr.readyState !== XMLHttpRequest.DONE) return;

        if (xhr.status === 200) {
          console.log(xhr.responseText);

          // Deserializing (String → Object)
          responseObject = JSON.parse(xhr.responseText);

          // JSON → HTML String
          let newContent = '<div id="tours"><h1>Guided Tours</h1><ul>';

          responseObject.tours.forEach(tour => {
            newContent += `<li class="${tour.region} tour">
                <h2>${tour.location}</h2>
                <span class="details">${tour.details}</span>
                <button class="book">Book Now</button>
              </li>`;
          });

          newContent += '</ul></div>';

          document.getElementById('content').innerHTML = newContent;
        } else {
          console.log(`[${xhr.status}] : ${xhr.statusText}`);
        }
      };
    </script>
  </body>
</html>
```

```
{
  "tours": [
    {
      "region": "usa",
      "location": "New York, USA",
      "details": "$1,899 for 7 nights"
    },
    {
      "region": "europe",
      "location": "Paris, France",
      "details": "$2,299 for 7 nights"
    },
    {
      "region": "asia",
      "location": "Tokyo, Japan",
      "details": "$3,799 for 7 nights"
    }
  ]
}
```

### 5.3 Load JSONP

요청에 의해 웹페이지가 전달된 서버와 동일한 도메인의 서버로부터 전달된 데이터는 문제없이 처리할 수 있음. 하지만 보안상의 이유로 다른 도메인으로 요청(크로스 도메인 요청)은 제한됨. 이것을 동일출처원칙이라 함

<div aling="center">
<img src="https://poiemaweb.com/img/cdr.jpg" width="50%" height="50%">
</div>

동일 출처원칙을 우회하는 방법은 3가지가 있음

1. 웹서버의 프록시 파일
   서버에 원격 서버로부터 데이터를 수집하는 별도의 기능을 추가하는 것임. 이를 프록시라 함

2. JSONP
   script 태그의 원본 주소에 대한 제약은 존재하지 않는데 이것을 이용해 다른 도메인의 서버에서 데이터를 수집하는 방법임. 자신의 서버에 함수를 정의하고 다른 도메인의 서버에 얻고자 하는 데이터를 인수로 하는 함수 호출문을 로드하는 것임

<div aling="center">
<img src="https://poiemaweb.com/img/comparison_between_ajax_and_jsonp.png" width="50%" height="50%">
</div>

```
<!-- 루트 폴더(webserver-express/public)/loadjsonp.html -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="https://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id="content"></div>
  <script>
    function showTours(data) {
      console.log(data); // data: object

      // JSON → HTML String
      let newContent = `<div id="tours">
          <h1>Guided Tours</h1>
        <ul>`;

      data.tours.forEach(tour => {
        newContent += `<li class="${tour.region} tour">
            <h2>${tour.location}</h2>
            <span class="details">${tour.details}</span>
            <button class="book">Book Now</button>
          </li>`;
      });

      newContent += '</ul></div>';

      document.getElementById('content').innerHTML = newContent;
    }
    </script>
    <script src='https://poiemaweb.com/assets/data/data-jsonp.js'></script>
    <!-- <script>
      showTours({
        "tours": [
          {
            "region": "usa",
            "location": "New York, USA",
            "details": "$1,899 for 7 nights"
          },
          {
            "region": "europe",
            "location": "Paris, France",
            "details": "$2,299 for 7 nights"
          },
          {
            "region": "asia",
            "location": "Tokyo, Japan",
            "details": "$3,799 for 7 nights"
          }
        ]
      });
    </script> -->
  </body>
</html>
```

```
// https://poiemaweb.com/assets/data/data-jsonp.js
showTours({
  "tours": [
    {
      "region": "usa",
      "location": "New York, USA",
      "details": "$1,899 for 7 nights"
    },
    {
      "region": "europe",
      "location": "Paris, France",
      "details": "$2,299 for 7 nights"
    },
    {
      "region": "asia",
      "location": "Tokyo, Japan",
      "details": "$3,799 for 7 nights"
    }
  ]
});
```

3. Cross-Origin Resource sharing

HTTP 헤더에 추가적으로 정보를 추가하여 브라우저와 서버가 서로 통신해야한다는 사실을 알게하는 방법임. W3C 명세에 포함되어 있지만 최신 브라우저에서만 동작하며 서버에 HTTP 헤더를 설정해주어야 함

Node.js로 구현한 서버의 경우 CORS package를 사용하면 간단하게 Cross-Origin Resource sharing을 구현할 수 있음

```
const express = require('express')
const cors = require('cors')
const app = express()

app.use(cors())

app.get('/products/:id', function (req, res, next) {
  res.json({msg: 'This is CORS-enabled for all origins!'})
})

app.listen(80, function () {
  console.log('CORS-enabled web server listening on port 80')
})
```
