## 1. 클로저의 개념

클로저는 자바스크립트 고유의 개념이 아니라 함수를 일급 객체로 취급하는 함수 프로그래밍 언어에서 사용되는 중요한 특성임.

MDN에서 정의한 클로저는 아래와 같음

> "A closure is the combination of a function and the lexical environment within which that function was declared."
> 클로저는 함수와 그 함수가 선언됐을 때의 렉시컬 환경(Lexical environment)과의 조합이다.

핵심 내용은 "함수가 선언되었을 때의 렉시컬 환경"임

```
function outerFunc() {
  var x = 10;
  var innerFunc = function () { console.log(x); };
  innerFunc();
}

outerFunc(); // 10
```

함수 outerFunc 내에서 내부함수 innerFunc가 선언되고 호출되었음. 이때 내부함수 innerFunc는 자신을 포함하고 있는 외부함수 outerFunc의 변수 x에 접근할 수 있음. 왜냐면 함수 innerFunc가 함수 outerFunc의 내부에 선언되었기 때문임

스코프는 함수를 호출할 떄가 아니라 함수를 어디에 선언하였는지에 따라서 결정됨. 이를 **렉시컬 스코핑**이라 함. 위 예제의 함수 innerFunc는 함수 outerFunc의 내부에서 선언되었기 때문에 함수 innerFunc의 상위 스코프는 함수 outerFunc임. 함수 innerFunc의 상위 스코프는 전역 스코프가 됌

함수 innerFunc가 함수 outerFunc의 내부에 선언된 내부함수이므로 함수 innerFunc는 자신이 속한 렉시컬 스코프(전역, 함수 outerFunc, 자신의 스코프)를 참조할 수 있음. 이를 실행 컨텍스트 관점에서 보자면 아래와 같음

내부 함수 innerFunc가 호출되면 자신의 실행 컨텍스트가 스택에 쌓이고 변수 객체와 스코프 체인 그리고 this에 바인딩할 객체가 결정됨. 이때 스코프 체인은 전역 스포크를 가리키는 전역 객체와 함수 outerFunc의 스코프를 가리키는 함수 outerFunc의 스코프를 가리키는 함수 outerFunc의 활성 객체 그리고 함수 자신의 스코프를 가리키는 활성 객체를 순차적으로 바인딩함. 스코프 체인이 바인딩한 객체가 바로 렉시컬 스코프의 실체임

내부 함수 innerFunc가 자신을 포함하고 있는 외부 함수 outerFunc의 변수 x에 접근할 수 있는 것 즉, 상위 스코프에 접근할 수 있는 것은 렉시컬 스코프의 레퍼런스를 차례대로 저장하고 있는 실행 컨텍스트의 스코프 체인을 자바스크립트 엔진이 검색하였기 때문에 가능한 것임. 자세히 설명하면 다음과 같음

1. innerFunc 함수 스코프(함수 자신의 스코프를 가리키는 활성 객체) 내에서 변수 x를 검색함 -> 검색 실패
2. innerFunc 함수를 포함하는 외부 함수 outerFunc의 스코프(함수 outerFunc의 스코프를 가리키는 함수 outerFunc의 활성객체)에서 변수 x 검색 -> 성공

아래의 예시와 같이 내부 함수 innerFunc를 함수 outerFunc 안에서 호출하는 것이 아니라 반환하도록 변경하면 어떻게 될까?

```
function outerFunc() {
  var x = 10;
  var innerFunc = function () { console.log(x); };
  return innerFunc;
}

/**
 *  함수 outerFunc를 호출하면 내부 함수 innerFunc가 반환됨
 *  그리고 함수 outerFunc의 실행 컨텍스트는 소멸함
 */
var inner = outerFunc();
inner(); // 10
```

위의 예시를 보다가 변수에 굳이 할당하지 않고 그냥 outerFunc()를 호출하면 되는게 아닌가 싶어서 콘솔을 찍어보았음

```
outerFunc()
```

내가 생각한 값은 10이었는데 함수 객체가 리턴됨
찾아보니 outerFunc()를 직접 호출하면 innerFunc를 리턴하는 함수 객체가 반환되고, 이 함수 객체는 호출되지 않았으므로 innerfunc의 스코프 체인에 있는 x변수에 접근할 수 없다고 함.
즉, outerFunc()를 호출하면 innerFunc를 반환하는 함수 객체가 리턴되고 이 함수 객체를 호출해야 비로소 내가 원하는 10이라는 값을 얻을 수 있는 것임

변수 x의 값에 접근하기 위해서 변수에 outerFunc를 할당해 innerFunc 함수 객체를 할당하고 이를 호출하면 그제서야 비로소 innerFunc()의 내부에서 변수 x의 값에 접근하여 변수 x의 값인 10이 출력되는 것이었음

```
var func = outerFunc();
func(); // 10

```

아니면 이렇게도 작성할 수 있음

```
outerFunc()()
```

outerFunc()가 innerFunc의 함수객체를 반환하고 ()를 한번 더 붙여 함수 객체를 호출해 10이라는 값을 얻을 수 있는 것임
근데 가독성이 별로니까 변수에 할당하자

---

위는 그냥 내가 예시 코드를 보다가 궁금해서 정리해놓은 것임

여튼 다시 본문으로 돌아가서 outer함수는 내부함수 innerFunc를 반환하고 생을 마감함. 즉, 함수 outerFunc는 실행된 이후 콜스택(실행 컨텍스트 스택)에서 제거되었으므로 함수 outerFunc의 변수 x 또한 더이상 유효하지 않게 되어 변수 x에 접근할 수 있는 방법은 달리 없어 보임. 하지만 위 코드의 실행결과는 변수 x값인 10임. 이미 라이프 사이클이 종료되어 실행 컨텍스트 스택에서 제거된 함수 outerFunc의 지역변수 x가 다시 부활이라도 한 듯이 동작하고 있음

이처럼 자신을 포함하고 있는 외부 함수보다 내부함수가 더 오래 유지되는 경우 외부 함수 밖에서 내부함수가 호출되더라도 외부함수의 지역 변수에 접근할 수 있는데 이를 **함수 클로저**라고 함

MDN에서 말하는 클로저의 정의에서 말하는 "함수"란 반환된 내부함수를 의미하고 "그 함수가 선언될 때의 렉시컬 환경"이란 내부 함수가 선언되었을 때 스코프를 의미함. 즉, **클로저는 반환된 내부함수가 자신이 선언되었을 때의 환경(렉시컬 환경)인 스코프를 기억하여 자신이 선언되었을 때의 환경(스코프) 밖에서 호출되어도 그 환경(스코프)에 접근할 수 있는 함수**를 말함.
좀 더 간단히 말하자면 **클로저는 자신이 생성될 때의 환경(렉시컬 환경)을 기억하는 함수**라고 말할 수 있음

클로저에 의해 참조되는 외부함수의 변수 즉, outerFunc 함수의 변수 x를 **자유변수**라 함. 클로저라는 이름은 자우변수에 함수가 닫혀있다는 의미로 의역하면 자유변수에 엮여있는 함수라는 뜻임.

실행 컨텍스트의 관점에서 설명하면 내부 함수가 유효한 상태에서 외부함수가 종료하여 외부함수의 실행 컨텍스트가 반환되어도 외부함수 실행 컨텍스트 내의 활성객체(변수, 함수, 선언 등의 정보를 가지고 있음)는 내부함수에 의해 참조되는 한 유효하며 내부함수가 스코프체인을 통해 참조할 수 있는 것을 의미함

즉, 외부함수가 이미 반환되었어도 외부함수 내의 변수는 이를 필요로 하는 내부함수가 하나 이상 존재하는 경우 계속 유지됨. 이때 내부함수가 외부함수에 있는 변수의 복사본이 아니라 실제 변수에 접근한다는 것에 주의해야함

<div align="center">
<img src="https://poiemaweb.com/img/closure.png" width="50%" height="50%">
</div>

## 2. 클로저의 활용

클로저는 자신이 생성될 때의 환경(렉시컬 환경)을 기억해야 하므로 메모리 차원에서 손해를 볼 수 있음. 하지만 유용하게 사용되는 상황이 있으므로 아래와 같은 상황에서 적극적으로 사용해보자

### 2.1 상태 유지

클로저가 가장 유용하게 사용되는 상황은 **현재 상태를 기억하고 변경된 최신 상태를 유지**하는 것임

```
<!DOCTYPE html>
<html>
<body>
  <button class="toggle">toggle</button>
  <div class="box" style="width: 100px; height: 100px; background: red;"></div>

  <script>
    var box = document.querySelector('.box');
    var toggleBtn = document.querySelector('.toggle');

    var toggle = (function () {
      var isShow = false;

      // ① 클로저를 반환
      return function () {
        box.style.display = isShow ? 'block' : 'none';
        // ③ 상태 변경
        isShow = !isShow;
      };
    })();

    // ② 이벤트 프로퍼티에 클로저를 할당
    toggleBtn.onclick = toggle;
  </script>
</body>
</html>
```

1. 즉시실행함수는 함수를 반환하고 즉시 소멸함. 즉시실행함수가 반환한 함수는 자신이 생성됐을 때의 렉시컬 환경(Lexical environment)에 속한 변수 isShow를 기억하는 클로저임. 클로저가 기억하는 변수 isShow는 box 요소의 표시 상태를 나타낸다.

2. 클로저를 이벤트 핸들러로서 이벤트 프로퍼티에 할당함. 이벤트 프로퍼티에서 이벤트 핸들러인 클로저를 제거하지 않는 한 클로저가 기억하는 렉시컬 환경의 변수 isShow는 소멸하지 않음. 다시 말해 현재 상태를 기억함.

3. 버튼을 클릭하면 이벤트 프로퍼티에 할당한 이벤트 핸들러인 클로저가 호출됨. 이때 .box 요소의 표시 상태를 나타내는 변수 isShow의 값이 변경됨. 변수 isShow는 클로저에 의해 참조되고 있기 때문에 유효하며 자신의 변경된 최신 상태를 게속해서 유지함.

이처럼 클로저는 현재 상태(위 예제의 경우 .box 요소의 표시 상태를 나타내는 isShow 변수)를 기억하고 이 상태가 변경되어도 최신 상태를 유지해야 하는 상황에 매우 유용함. 만약 자바스크립트에 클로저라는 기능이 없다면 상태를 유지하기 위해 전역 변수를 사용할 수 밖에 없음. 전역 변수는 언제든지 누구나 접근할 수 있고 변경할 수 있기 때문에 많은 부작용을 유발해 오류의 원인이 되므로 사용을 억제해야 함

### 2.2 전역 변수의 사용 억제

```
<!DOCTYPE html>
<html>
<body>
  <p>전역 변수를 사용한 Counting</p>
  <button id="inclease">+</button>
  <p id="count">0</p>
  <script>
    var incleaseBtn = document.getElementById('inclease');
    var count = document.getElementById('count');

    // 카운트 상태를 유지하기 위한 전역 변수
    var counter = 0;

    function increase() {
      return ++counter;
    }

    incleaseBtn.onclick = function () {
      count.innerHTML = increase();
    };
  </script>
</body>
</html>
```

버튼이 클릭될 때마다 클릭한 횟수가 누적되어 화면에 표시되는 카운터를 만드는 예시임.
이 예제에서 클릭된 횟수가 바로 유지해야할 상태임

하지만 위의 코드는 오류를 발생시킬 가능성을 내포하고 있는 코드임. increase 함수는 호출되기 직전에 전역변수 counter의 값이 반드시 0이여야 제대로 동작함. 하지만 counter는 전역변수이기 때문에 언제든지 누구나 접근할 수 있음. 이는 의도치 않게 값이 변경될 수 있다는 것을 의미함. 만약 누군가에 의해 의도치 않게 전역 변수 counter의 값이 변경됐다면 이는 오류로 이어짐. 변수 counter를 관리하는 increase 함수가 관리하는 것이 바람직함. 전역 변수 counter를 increase 함수의 지역변수로 바꾸어 의도치 않은 상태 변경을 방지하면 다음과 같음

```
<!DOCTYPE html>
<html>
<body>
  <p>지역 변수를 사용한 Counting</p>
  <button id="inclease">+</button>
  <p id="count">0</p>
  <script>
    var incleaseBtn = document.getElementById('inclease');
    var count = document.getElementById('count');

    function increase() {
      // 카운트 상태를 유지하기 위한 지역 변수
      var counter = 0;
      return ++counter;
    }

    incleaseBtn.onclick = function () {
      count.innerHTML = increase();
    };
  </script>
</body>
</html>
```

전역변수를 지역변수로 변경하여 의도치 않은 상태 변경을 방지함. 하지만 increase 함수가 호출될 때마다 지역변수 counter를 0으로 초기화하기 때문에 언제나 1이 표시됨. 즉, 변경된 이전 상태를 기억하지 못함. 이전 상태를 기억하도록 클로저를 사용해 이 문제를 해결하면 다음과 같음

```
<!DOCTYPE html>
<html>
  <body>
  <p>클로저를 사용한 Counting</p>
  <button id="inclease">+</button>
  <p id="count">0</p>
  <script>
    var incleaseBtn = document.getElementById('inclease');
    var count = document.getElementById('count');

    var increase = (function () {
      // 카운트 상태를 유지하기 위한 자유 변수
      var counter = 0;
      // 클로저를 반환
      return function () {
        return ++counter;
      };
    }());

    incleaseBtn.onclick = function () {
      count.innerHTML = increase();
    };
  </script>
</body>
</html>
```

스크립트가 실행되면 즉시실행함수가 호출되고 변수 increase에는 함수 function() {return ++counter;} 가 할당됨. 이 함수는 자신이 생성 되었을 때 렉시컬 환경을 기억하는 클로저임. 즉시실행함수는 호출된 이후 소명되지만 즉시실행함수가 반환한 함수는 변수 increase에 할당되어 increase 버튼을 클릭하면 클릭 이벤트 핸들러 내부에서 호출됨. 이때 클로저인 이 함수는 자신이 선언됐을 때의 렉시컬 환경인 즉시실행함수의 스코프에 속한 지역변수 counter를 기억함. 따라서 즉시실행함수의 변수 counter에 접근할 수 있고 변수 counter는 자신을 참조하는 함수가 소멸될 때까지 유지됨.
즉시 실행함수는 한 번만 실행되므로 increase가 호출될 때마다 변수 counter가 재차 초기화될 일은 없을 것임. 변수 counter는 외부에서 직접 접근할 수 없는 private 변수이므로 전역 변수를 사용했을 때와 같이 의도되지 않은 변경을 걱정할 필요도 없기 때문에 보다 안정적인 프로그래밍이 가능함

> 변수의 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적인 원인이 될 수 있음. 상태 변경이나 가변 데이터를 피하고 **불변성을 지향**하는 함수 프로그래밍에서 **사이드 이펙트를 최대한 억제**하여 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저는 적극적으로 사용됨

아래는 함수 프로그램에서 클로저를 활용하는 간단한 예임

```
// 함수를 인자로 전달받고 함수를 반환하는 고차 함수
// 이 함수가 반환하는 함수는 클로저로서 카운트 상태를 유지하기 위한 자유 변수 counter을 기억한다.

function makeCounter(predicate) {
  // 카운트 상태를 유지하기 위한 자유 변수
  var counter = 0;
  // 클로저를 반환
  return function () {
    counter = predicate(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 함수로 함수를 생성한다.
// makeCounter 함수는 보조 함수를 인자로 전달받아 함수를 반환한다
const increaser = makeCounter(increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다.
const decreaser = makeCounter(decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

함수 makeCounter는 보조 함수를 인자로 전달받고 함수를 반환하는 고차함수임. 함수 makeCounter가 반환하는 함수는 자신이 생성됐을 때의 렉시컬 환경인 함수 makeCounter의 스코프에 속한 변수 counter를 기억하는 클로저임. 함수 makeCounter는 인자로 전달받은 보조함수를 합성하여 자신이 반환하는 함수의 동작을 변경할 수 있음. 이때 주의해야할 점은 함수 makeCounter를 호출해 함수를 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다는 것임. 이는 함수를 호출하면 그때마다 새로운 렉시컬 환경이 생성되기 때문임. 위의 예시에서 변수 increaser와 변수 decreaser에 할당된 함수는 각각 자신만의 독립된 렉시컬 환경을 갖기 때문에 카운트를 유지하기 위한 자유 변수 counter응 공유하지 않아 카운터의 증감이 연동하지 않음. 따라서 독립된 카운터가 아니라 연동하여 증감이 가능한 카운터를 만드려면 렉시컬 환경을 공유하는 클로저를 만들어야 함

아래와 같이 렉시컬 환경을 공유하는 클로저를 만들어 증감이 연동되게 할 수 있음

```
function makeCounter() {
  // 카운트 상태를 유지하기 위한 자유 변수
  var count = 0;
  // 클로저를 반환
  return {
    increment: function() {
      return ++count;
    },
    decrement: function() {
      return --count;
    }
  };
}

// 함수로 함수를 생성한다.
// makeCounter 함수가 클로저를 반환하는 함수를 반환하는 것을 이용하여 카운트 상태를 공유하는 클로저를 만든다.
const sharedCounter1 = makeCounter();

console.log(sharedCounter1.increment()); // 1
console.log(sharedCounter1.increment()); // 2
console.log(sharedCounter1.decrement()); // 1
console.log(sharedCounter1.decrement()); // 0

// sharedCounter1과 동일한 렉시컬 환경을 공유하기 때문에 카운트 상태가 연동된다.
const sharedCounter2 = makeCounter();

console.log(sharedCounter2.increment()); // 1
console.log(sharedCounter1.increment()); // 1
console.log(sharedCounter2.decrement()); // 0
console.log(sharedCounter1.decrement()); // -1
```

### 2.3 정보 은닉

```
function Counter() {
  // 카운트를 유지하기 위한 자유 변수
  var counter = 0;

  // 클로저
  this.increase = function () {
    return ++counter;
  };

  // 클로저
  this.decrease = function () {
    return --counter;
  };
}

const counter = new Counter();

console.log(counter.increase()); // 1
console.log(counter.decrease()); // 0
```

생성자 함수 Counter는 increase, decrease 메소드를 갖는 인스턴스를 생성함. 이 메소드들은 모두 자신이 생성됐을 때의 렉시컬 환경인 생성자 함수 Counter의 스코프에 속한 변수 counter를 기억하는 클로저이며 렉시컬 환경을 공유함. 생성자 함수가 생성한 객체의 메소드는 객체의 프로퍼티에만 접근할 수 있는 것이 아니며 자신이 기억하는 렉시컬 환경의 변수에도 접근할 수 있음.
이때 생성자 함수 Counter의 변수 counter는 this에 바인딩된 프로퍼티가 아니라 변수임. counter가 this에 바인딩된 프로퍼티라면 생성자 함수 Counter가 생성한 인스턴스를 통해 외부에서 접근이 가능한 public 프로퍼티가 되지만 생성자 함수 Counter에서 접근할 수 없음. 하지만 생성자 함수 Counter가 생성한 인스턴스 메소드인 increase, decrease는 클로저이기 때문에 자신이 생성됐을 때의 렉시컬 환경인 생성자 함수 Counter의 변수 counter에 접근할 수 있음. 이러한 클로저의 특징을 사용해 클래스 기반의 언어의 private 키워드를 흉내낼 수 있음.

### 2.4 자주 발생하는 실수

아래는 클로저를 사용할 때 자주 발생하는 실수에 관련된 예시들임

```
var arr = [];

for (var i = 0; i < 5; i++) {
  arr[i] = function () {
    return i;
  };
}

for (var j = 0; j < arr.length; j++) {
  console.log(arr[j]());
}
```

배열 arr에 5개의 함수가 할당되고 각각의 함수는 순차적으로 0, 1, 2, 3, 4를 반환할 것으로 기대하겠지만 결과는 그렇지않다. for 문에서 사용한 변수 i는 전역 변수이기 때문이다. 이러한 문제를 클로저를 사용해 바르게 동작하는 코드로 만들어보자.

```
var arr = [];

for (var i = 0; i < 5; i++){
  arr[i] = (function (id) { // 2번
    return function () {
      return id; // 3번
    };
  }(i)); // 1번
}

for (var j = 0; j < arr.length; j++) {
  console.log(arr[j]());
}
```

1. 배열 arr에는 즉시실행함수에 의해 함수가 반환됨

2. 이때 즉시실행함수는 i를 인자로 전달받고 매개변수 id에 할당한 후 내부 함수를 반환하고 life-cycle이 종료됨. 매개변수 id는 자유변수가 됨

3. 배열 arr에 할당된 함수는 id를 반환함. 이때 id는 상위 스코프의 자유변수이므로 그 값이 유지됨

위 예제는 자바스크립트의 함수 레벨 스코프 특성으로 인해 for 루프의 초기문에서 사용된 변수의 스코프가 전역이 되기 때문에 발생하는 현상임. ES6의 let 키워드를 사용하면 이와 같은 문제는 해결됨

```
const arr = [];

for (let i = 0; i < 5; i++) {
  arr[i] = function () {
    return i;
  };
}

for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]());
}
```

또는 함수형 프로그래밍 기법인 고차 함수를 사용하는 방법이 있음. 이 방법은 변수와 반복문의 사용을 억제할 수 있기 때문에 때문에 애플리케이션의 오류를 줄이고 가독성을 좋게 만듦

```
const arr = new Array(5).fill();

arr.forEach((v, i, array) => array[i] = () => i);

arr.forEach(f => console.log(f()));
```
