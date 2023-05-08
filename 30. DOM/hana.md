## 1. DOM

텍스트 파일로 만들어져 있는 웹 문서를 브라우저에 렌더링하려면 웹 문서를 브라우저가 이해할 수 있는 구조로 메모리에 올려야 함. 브라우저의 렌더링 엔진은 웹 문서를 로드한 후, 파싱하여 웹 문서를 브라우저가 이해할 수 있는 구조로 구성하여 메모리에 적재하는데 이를 DOM이라 함.
즉 모든 요소와 요소의 어트리뷰트, 텍스트를 각각의 객체로 만들고 이를 객체를 모녀 관계를 표현할 수 있는 트리 구조로 구성한 것이 DOM임. 이 DOM은 자바스크립트를 통해 동적으로 변경할 수 있으며 변경된 DOM은 렌더링에 반영됨

<div align="center">
<img src="https://poiemaweb.com/img/client-server.png" width="50%" height="50%">
</div>

이러한 동적 변경을 위해 DOM은 프로그래밍 언어가 자신에 접근하고 수정할 수 있는 방법을 제공하는데 일반적으로 프로퍼티와 메소드를 갖고 있는 자바스크립트 객체로 제공됨. 이를 DOM API(Application Programming Interface)라고 부름. 즉 정적인 웹페이지에 접근해 동적으로 웹페이지를 변경하기 위한 유일한 방법은 메모리상에 존재하는 DOM을 변경하는 것이고, 이떄 필요한 것이 DOM에 접근하고 변경하는 프로퍼티와 메소드의 집한인 DOM API임
DOM은 HTML, ECMAScript에서 정의한 표준이 아닌 변개의 W3C의 공식 표준이며 플랫폼/프로그래밍 언어 중립적임. DOM은 다음 두 가지 기능을 담당함

> **HTML문서에 대한 모델 구성**
> 브라우저는 HTML 문서를 로드한 후 해당 문서에 대한 모델을 메모리에 생성함. 이때 모델은 객체의 트리로 구성되는데 이것을 DOM tree라 함

> **HTML문서 내의 각 요소에 접근 / 수정**
> DOM은 모델 내의 각 객체에 접근하고 수정할 수 있는 프로퍼티와 메소드를 제공함. DOM이 수정되면 브라우저를 통해 사용자가 보게 될 내용 또한 변경됨

## 2. DOM tree

DOM tree는 브라우저가 HTML 문서를 로드한 후 파싱하여 생성하는 모델을 의미함
객체의 트리로 구조화되어 있기 때문에 DOM tree라 부름

```
<!DOCTYPE html>
<html>
  <head>
    <style>
      .red  { color: #ff0000; }
      .blue { color: #0000ff; }
    </style>
  </head>
  <body>
    <div>
      <h1>Cities</h1>
      <ul>
        <li id="one" class="red">Seoul</li>
        <li id="two" class="red">London</li>
        <li id="three" class="red">Newyork</li>
        <li id="four">Tokyo</li>
      </ul>
    </div>
  </body>
</html>
```

<div align="center">
<img src="https://poiemaweb.com/img/dom-tree.png" width="50%" height="50%">

</div>

DOM에서는 모든 요소, 어트리뷰트, 텍스트는 하나의 객체이며 Document 객체의 자식임. 요소의 중첩 관계는 객체의 트리로 구조화하여 모녀관계를 표현함. DOM tree의 진입점(entry point)는 document 객체이며 최종점은 요소의 텍스트를 나타내는 객체임

DOM tree는 네 종류의 노드로 구성됨

> **문서노드(document node)**
> 트리의 최상위에 존재하며 각각 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 함. 즉, DOM tree에 접근하기 위한 시작점임

> **요소 노드(element node)**
> 요소 노드는 HTML 요소를 표현함. HTML 요소는 중접에 의해 모녀 관계를 가지며 이 모녀 관계를 통해 정보를 구조화함. 따라서 요소 노드는 문서의 구조를 서술한다고 말할 수 있음. 어트리뷰트, 텍스트 노드에 접근하려면 먼저 요소 노드를 찾아 접근해야함. 모든 요소 노드는 요소별 특성을 표현하기 위해 HTMLElement 객체를 상속한 객체로 구성함

> **어트리뷰트 노드(Attribute node)**
> 어트리뷰트 노드는 HTML 요소의 어트리뷰트를 표현함. 어트리뷰트 노드는 해당 어트리뷰트가 지정된 요소의 자식이 아니라 해당 요소의 일부로 표현됨. 따라서 해당 요소 노드를 찾아 접근하면 어트리뷰트를 참조, 수정할 수 있음

> **텍스트 노드(text node)**
> 텍스트 노드는 HTML 요소의 텍스트를 표현함. 텍스트 노드는 요소 노드의 자식이며 자신의 자식 노드를 가질 수 없음. 즉 텍스트 노드는 DOM tree의 최종단임

<div align="center">
<img src="https://poiemaweb.com/img/HTMLElement.png" width="50%" height="50%">

</div>

DOM을 통해 웹페이지를 조작하기 위해서는 다음과 같은 수순이 필요함

- 조작하고자하는 요소를 선택, 탐색함
- 선택된 요소의 콘텐츠 또는 어트리뷰트를 조작함

자바스크립트는 이것에 필요한 수단(API)를 제공함

## 3. DOM Query / Traversing (요소에의 접근)

### 3.1 하나의 요소 노드 선택(DOM Query)

<div align="center">
<img src="https://poiemaweb.com/img/select-an-individual-element-node.png" width="50%" height="50%">
</div>

**document.getElementById(id)**

- id 어트리뷰트 값으로 요소 노드를 한 개 선택함. 복수개가 선택된 경우 첫 번째 요소만 반환함
- return : HTMLElement를 상속받은 객체
- 모든 브라우저에서 동작

```
// id로 하나의 요소를 선택한다.
const elem = document.getElementById('one');

// 클래스 어트리뷰트의 값을 변경함
elem.className = 'blue';

// 그림: DOM tree의 객체 구성 참고
console.log(elem); // <li id="one" class="blue">Seoul</li>
console.log(elem.__proto__);           // HTMLLIElement
console.log(elem.__proto__.__proto__); // HTMLElement
console.log(elem.__proto__.__proto__.__proto__);           // Element
console.log(elem.__proto__.__proto__.__proto__.__proto__); // Node
```

**document.querySelector(cssSelector)**

- css 셀럭터를 사용해 요소 노드를 한 개 선택함. 복수개가 선택된 경우 첫 번째 요소만 반환함
- return : HTMLElement를 상속받은 객체
- IE8이상의 브라우저세어 동작

```
// CSS 셀렉터를 이용해 요소를 선택
const elem = document.querySelector('li.red');

// 클래스 어트리뷰트의 값을 변경
elem.className = 'blue';
```

### 3.2 여러 개의 요소 노드 선택(DOM Query)

<div align="center">
<img src="https://poiemaweb.com/img/select-multiful-elements.png" width="50%" height="50%">
</div>

**document.getElementsByClassName(class)**

- class 어트리뷰트 값으로 요소 노드를 모두 선택함. 공백으로 구분해 여러 개의 class를 지정할 수 있음
- return : HTMLCollection(live)
- IE9 이상의 브라우저에서 동작

```
// HTMLCollection을 반환
const elems = document.getElementsByClassName('red');

for (let i = 0; i < elems.length; i++) {

  // 클래스 어트리뷰트의 값을 변경
  elems[i].className = 'blue';
}
```

위 예제를 실행해 보면 예상대로 동작하지 않음(두번째 요소만 클래스 변경이 되지 않음)

getElementsByClassName 메소드의 반환값은 HTMLCollection임. 이것은 반환값이 복수인 경우, HTMLElement의 리스트를 담아 반환하기 위한 객체로 배열과 비슷한 사용법을 가지고 있지만 배열은 아닌 유사배열(array-like object)임. 또한 HTMLCollection은 실시간으로 Node의 상태 변경을 반영함(live HTMLCollection)

위 예제가 예상대로 동작하지 않은 이유는 아래와 같음

1. elems.length는 3이므로 3번의 loop가 실행됨

2. i가 0일때, elems의 첫 요소(li#one.red)의 class 어트리뷰트의 값이 className 프로퍼티에 의해 red에서 blue로 변경됨. 이때 elems는 실시간으로 Node의 상태 변경을 반영하는 HTMLCollection 객체임. elems의 첫 요소는 li#one.red에서 li#one.blue로 변경되었으므로 getElementsByClassName 메소드의 인자로 지정한 선택 조건(‘red’)과 더이상 부합하지 않게 되어 반환값에서 실시간으로 제거됨

3. i가 1일때, elems에서 첫째 요소는 제거되었으므로 elems[1]은 3번째 요소(li#three.red)가 됨. li#three.red의 class 어트리뷰트 값이 blue로 변경되고 마찬가지로 HTMLCollection에서 제외됨

4. i가 2일때, HTMLCollection의 1,3번째 요소가 실시간으로 제거되었으므로 2번째 요소(li#two.red)만 남음. 이때 elems.length는 1이므로 for 문의 조건식 i < elems.length가 false로 평가되어 반복을 종료함. 따라서 elems에 남아 있는 2번째 li 요소(li#two.red)의 class 값은 변경되지 않음

이처럼 HTMLCollection는 실시간으로 Node의 상태 변경을 반영하기 때문에 loop가 필요한 경우 주의가 필요함. 아래와 같은 방법으로 회피할 수 있음

- 반복문을 역방향으로 돌리기

```
const elems = document.getElementsByClassName('red');

for (let i = elems.length - 1; i >= 0; i--) {
  elems[i].className = 'blue';
}
```

- while 반복문 사용하기. 이때 elems에 요소가 남아있지 않을 때까지 무한 반복하기 위해 index는 0으로 고정함

```
const elems = document.getElementsByClassName('red');

let i = 0;
while (elems.length > i) { // elems에 요소가 남아 있지 않을 때까지 무한반복
  elems[i].className = 'blue';
  // i++;
}
```

- HTMLCollection을 배열로 변경함. 이 방법을 권장함

```
const elems = document.getElementsByClassName('red');

// 유사 배열 객체인 HTMLCollection을 배열로 변환함
// 배열로 변환된 HTMLCollection은 더 이상 live하지 않음
console.log([...elems]); // [li#one.red, li#two.red, li#three.red]

[...elems].forEach(elem => elem.className = 'blue');
```

- querySelectorAll 메소드를 사용해 HTMLCollection(live)가 아닌 NodeList(non-live)를 반환하게 함

```
// querySelectorAll는 Nodelist(non-live)를 반환
const elems = document.querySelectorAll('.red');

[...elems].forEach(elem => elem.className = 'blue');
```

**document.getElementsByTagName(tagName)**

- 태그명으로 요소 노드를 모두 선택함
- return : HTMLCollection(live)
- 모든 브라우저에서 동작

```
// HTMLCollection을 반환
const elems = document.getElementsByTagName('li');

[...elems].forEach(elem => elem.className = 'blue');
```

**document.querySelectorAll(selector)**

- 지정된 css 선택자를 사용해 요소 노드를 모두 선택함
- return : NodeList(non-live)
- IE8 이상의 브라우저에서 동작

```
// Nodelist를 반환
const elems = document.querySelectorAll('li.red');

[...elems].forEach(elem => elem.className = 'blue');
```

### 3.3 DOM Traversing(탐색)

<div align="center">
<img src="https://poiemaweb.com/img/traversing.png" width="50%" height="50%">
</div>

**parentNode**

- 부모 노드를 탐색함
- return : HTMLElement를 상속받은 객체
- 모든 브라우저에서 동작

```
const elem = document.querySelector('#two');

elem.parentNode.className = 'blue';
```

**firstChild, lastChild**

- 자식 노드를 탐색함
- return : HTMLElement를 상속받은 객체
- IE9 이상의 브라우저에서 동작

```
const elem = document.querySelector('ul');

// first Child
elem.firstChild.className = 'blue';

// last Child
elem.lastChild.className = 'blue';
```

위 예시를 실행해보면 예상대로 동작하지 않음. 그 이유는 IE를 제외한 대부분의 브라우저들을 요소 사이의 공백 또는 줄바꿈 문자를 텍스트 노드 취급하기 때문임. 이것을 회피하기 위해서는 아래와 같이 HTML의 공백을 제거하거나 jQuery: .prev()와 jQuery: .next()를 사용한다.

```
<ul><li
  id='one' class='red'>Seoul</li><li
  id='two' class='red'>London</li><li
  id='three' class='red'>Newyork</li><li
  id='four'>Tokyo</li></ul>
```

또는 firstElementChild, lastElementChild를 사용할 수도 있음. 이 두 가지 프로퍼티는 모든 IE9이상에서 정상 작동함

```
const elem = document.querySelector('ul');

// first Child
elem.firstElementChild.className = 'blue';

// last Child
elem.lastElementChild.className = 'blue';
```

**hasChildNode()**

- 자식 노드가 있는지 확인하고 boolean 값을 반환함
- return : boolean 값
- 모든 브라우저에서 동작

**childNodes**

- 자식 노드의 컬렉션을 반환함. **텍스트 요소를 포함한 모든 자식 요소를 반환함**
- return : NodeList(non-live)
- 모든 브라우저에서 동작

**children**

- 자식 노드의 컬렉션을 반환함. **자식 요소 중에서 Element type 요소만을 반환함**
- return : HTMLCollection(live)
- IE9 이상의 브라우저에서 동작

```
const elem = document.querySelector('ul');

if (elem.hasChildNodes()) {
  console.log(elem.childNodes);
  // 텍스트 요소를 포함한 모든 자식 요소를 반환
  // NodeList(9) [text, li#one.red, text, li#two.red, text, li#three.red, text, li#four, text]

  console.log(elem.children);
  // 자식 요소 중에서 Element type 요소만을 반환
  // HTMLCollection(4) [li#one.red, li#two.red, li#three.red, li#four, one: li#one.red, two: li#two.red, three: li#three.red, four: li#four]
  [...elem.children].forEach(el => console.log(el.nodeType)); // 1 (=> Element node)
}
```

**previousSibling, nextSibling**

- 형제 노드를 탐색함. text node를 포함한 모든 형제 노드를 탐색함
- return : HTMLElement를 상속받은 객체
- 모든 브라우저에서 동작

**previousElementSibling, nextElementSibling**

- 형제 노드를 탐색함. 형제 노드 중에서 Element type 요소만을 탐색함
- return : HTMLElement를 상속받은 객체
- IE9 이상의 브라우저에서 동작

```
const elem = document.querySelector('ul');

elem.firstElementChild.nextElementSibling.className = 'blue';
elem.firstElementChild.nextElementSibling.previousElementSibling.className = 'blue';
```

## 4. DOM Manipulation(조작)

### 4.1 텍스트 노드에의 접근/수정

<div align="center">
<img src="https://poiemaweb.com/img/nodeValue.png" width="50%" height="50%">
</div>

요소의 텍스트는 텍스트 노드에 저장되어 있음. 텍스트 노드에 접근하려면 아래와 같은 수순이 필요함

1. 해당 텍스트 노드의 부모 노드를 선택함. 텍스트 노드는 요소 노드의 자식임
2. firstChild 프로퍼티를 사용해 텍스트 노드를 탐색함
3. 텍스트 노드의 유일한 프로퍼티(nodeValue)를 이용해 텍스트를 취득함
4. nodeValue를 사용해 텍스트를 수정함

**nodeValue**

- 노드의 값을 반환함
- return : 텍스트 노드 같은 경우는 문자열, 요소 노드의 경우 null 반환
- IE6 이상의 브라우저에서 동작함

nodeName, nodeType을 통해 노드의 정보를 취득할 수 있음

```
// 해당 텍스트 노드의 부모 요소 노드를 선택함
const one = document.getElementById('one');
console.dir(one); // HTMLLIElement: li#one.red

// nodeName, nodeType을 통해 노드의 정보를 취득할 수 있음
console.log(one.nodeName); // LI
console.log(one.nodeType); // 1: Element node

// firstChild 프로퍼티를 사용하여 텍스트 노드를 탐색함
const textNode = one.firstChild;

// nodeName, nodeType을 통해 노드의 정보를 취득할 수 있음
console.log(textNode.nodeName); // #text
console.log(textNode.nodeType); // 3: Text node

// nodeValue 프로퍼티를 사용하여 노드의 값을 취득함
console.log(textNode.nodeValue); // Seoul

// nodeValue 프로퍼티를 이용하여 텍스트를 수정함
textNode.nodeValue = 'Pusan';
```

### 4.2 어트리뷰트 노드에의 접근/수정

<div align="center">
<img src="https://poiemaweb.com/img/nodeValue.png" width="50%" height="50%">
</div>

어트리뷰트 노드를 조작할 때 다음 프로퍼티 또는 메소드를 사용할 수 있음

**className**

- class 어트리뷰트의 값을 취득 또는 변경함. className 프로퍼티에 값을 할당하는 경우 class 어트리뷰트가 존재하지 않으면 class 어트리뷰트를 생성하고 지정된 값을 설정함. class 어트리뷰트 값이 여러 개일 경우, 공백으로 구분된 문자열이 반환되므로 string 메소드 split(' ')을 사용해 배열로 변경해 사용함
- 모든 브라우저에서 동작함

**classList**

- add, remove, item, toggle, contains, replace 메소드를 제공함
- IE10 이상의 브라우저에서 동작함

```
const elems = document.querySelectorAll('li');

// className
[...elems].forEach(elem => {
  // class 어트리뷰트 값을 취득하여 확인
  if (elem.className === 'red') {
    // class 어트리뷰트 값을 변경함
    elem.className = 'blue';
  }
});

// classList
[...elems].forEach(elem => {
  // class 어트리뷰트 값 확인
  if (elem.classList.contains('blue')) {
    // class 어트리뷰트 값 변경함
    elem.classList.replace('blue', 'red');
  }
});
```

**id**

- id 어트리뷰트의 값을 취득 또는 변경함. id 프로퍼티에 값을 할당하는 경우 id 어트리뷰트가 존재하지 않으면 id 어트리뷰트를 생성하고 지정된 값을 설정함
- 모든 브라우저에서 동작함

```
// h1 태그 요소 중 첫번째 요소를 취득
const heading = document.querySelector('h1');

console.dir(heading); // HTMLHeadingElement: h1
console.log(heading.firstChild.nodeValue); // Cities

// id 어트리뷰트의 값을 변경.
// id 어트리뷰트가 존재하지 않으면 id 어트리뷰트를 생성하고 지정된 값을 설정
heading.id = 'heading';
console.log(heading.id); // heading
```

**hasAttribute(attribute)**

- 지정한 어트리뷰트를 가지고 있는지 검사함
- return : Boolean
- IE8 이상의 브라우저에서 동작함

**getAttribute(attribute)**

- 어트리뷰트의 값을 취득함
- return : 문자열
- 모든 브라우저에서 동작함

**setAttribute(attribute, value)**

- 어트리뷰트와 어트리뷰트 값을 설정함
- return : undefined
- 모든 브라우저에서 동작함

**removeAttribute(attribute)**

- 지정한 어트리뷰트를 제거함
- return : undefined
- 모든 브라우저에서 동작함

```
<!DOCTYPE html>
<html>
  <body>
  <input type="text">
  <script>
  const input = document.querySelector('input[type=text]');
  console.log(input);

  // value 어트리뷰트가 존재하지 않으면
  if (!input.hasAttribute('value')) {
    // value 어트리뷰트를 추가하고 값으로 'hello!'를 설정
    input.setAttribute('value', 'hello!');
  }

  // value 어트리뷰트 값을 취득
  console.log(input.getAttribute('value')); // hello!

  // value 어트리뷰트를 제거
  input.removeAttribute('value');

  // value 어트리뷰트의 존재를 확인
  console.log(input.hasAttribute('value')); // false
  </script>
  </body>
</html>
```

### 4.3 HTML 콘텐츠 조작(Manipulation)

<div align="center">
<img src="https://poiemaweb.com/img/innerHTML.png" width="50%" height="50%">
</div>

HTML 콘텐츠를 조작하기 위해 아래의 프로퍼티 또는 메소드를 사용할 수 있음. 마크업이 포함된 콘텐츠를 추가하는 행위는 **크로스 스크립팅 공격(XSS)**에 취약하므로 주의가 필요함

**textContent**

- 요소의 텍스트 콘텐츠를 취득 또는 변경함. 이때 마크업은 무시됨. textContent를 통해 요소에 새로운 텍스트를 할당하면 텍스트를 변경할 수 있음. 이때 순수한 텍스트만 지정해야하며 마크업을 포함시키면 문자열로 인식되어 그대로 출력됨
- IE9 이상의 브라우저에서 동작함

```
<!DOCTYPE html>
<html>
  <head>
    <style>
      .red  { color: #ff0000; }
      .blue { color: #0000ff; }
    </style>
  </head>
  <body>
    <div>
      <h1>Cities</h1>
      <ul>
        <li id="one" class="red">Seoul</li>
        <li id="two" class="red">London</li>
        <li id="three" class="red">Newyork</li>
        <li id="four">Tokyo</li>
      </ul>
    </div>
    <script>
    const ul = document.querySelector('ul');

    // 요소의 텍스트 취득
    console.log(ul.textContent);
    /*
            Seoul
            London
            Newyork
            Tokyo
    */

    const one = document.getElementById('one');

    // 요소의 텍스트 취득
    console.log(one.textContent); // Seoul

    // 요소의 텍스트 변경
    one.textContent += ', Korea';

    console.log(one.textContent); // Seoul, Korea

    // 요소의 마크업이 포함된 콘텐츠 변경.
    one.textContent = '<h1>Heading</h1>';

    // 마크업이 문자열로 표시됨
    console.log(one.textContent); // <h1>Heading</h1>
    </script>
  </body>
</html>
```

**innerText**

- innerText 프로퍼티를 사용해도 요소의 텍스트 콘텐츠에만 접근할 수 있음. 하지만 아래의 이유로 사용하지 않는 것이 좋음

  - 비표준임
  - css에 순종적임. 예를 들어 css에 의해 visibility:hidden으로 설정되어 있다면 텍스트가 반환되지 않음
  - css를 고려해야하므로 textContent 프로퍼티보다 느림

**innerHTML**

- 해당 요소의 모든 자식 요소를 포함하는 모든 콘텐츠를 하나의 문자열로 취득할 수 있음. 이 문자열은 마크업을 포함함

```
const ul = document.querySelector('ul');

// innerHTML 프로퍼티는 모든 자식 요소를 포함하는 모든 콘텐츠를 하나의 문자열로 취득할 수 있음. 이 문자열은 마크업을 포함함
console.log(ul.innerHTML);

// IE를 제외한 대부분의 브라우저들은 요소 사이의 공백 또는 줄바꿈 문자를 텍스트 노드로 취급함
/*
        <li id="one" class="red">Seoul</li>
        <li id="two" class="red">London</li>
        <li id="three" class="red">Newyork</li>
        <li id="four">Tokyo</li>
*/
```

innerHTML 프로퍼티를 사용해 마크업이 포함된 새로운 콘텐츠를 지정하면 새로운 요소를 DOM에 추가할 수 있음

```
const one = document.getElementById('one');

// 마크업이 포함된 콘텐츠 취득
console.log(one.innerHTML); // Seoul

// 마크업이 포함된 콘텐츠 변경
one.innerHTML += '<em class="blue">, Korea</em>';

// 마크업이 포함된 콘텐츠 취득
console.log(one.innerHTML); // Seoul <em class="blue">, Korea</em>
```

위와 같이 마크업이 포함된 콘텐츠를 추가하는 것은 크로스 스크립팅 공격에 취약함

```
// 크로스 스크립팅 공격 사례

// 스크립트 태그를 추가하여 자바스크립트가 실행되도록 함
// HTML5에서 innerHTML로 삽입된 <script> 코드는 실행되지 않음
// 크롬, 파이어폭스 등의 브라우저나 최신 브라우저 환경에서는 작동하지 않을 수도 있음
elem.innerHTML = '<script>alert("XSS!")</script>';

// 에러 이벤트를 발생시켜 스크립트가 실행되도록 함
// 크롬에서도 실행됨
elem.innerHTML = '<img src="#" onerror="alert(\'XSS\')">';
```

### 4.4 DOM 조작 방식

innerHTML 프로퍼티를 사용하지 않고 새로운 콘텐츠를 수가할 수 있는 방법은 DOM을 직접 조작하는 것임. 한 개의 요소를 추가하는 경우 사용함. 방법은 다음과 같음

1. 요소 노드 생성 `createElement()` 메소드를 사용해 새로운 요소 노드를 생성함. `createElement()`에 인자로 태그 이름을 전달함
2. 텍스트 노드 생성 `createTextNode()` 메소드를 사용해 새로운 텍스트 노드를 생성함. 경우에 따라 생략될 수 있지만 생략하는 경우 콘텐츠가 비어있는 요소가 됨
3. 생성된 요소를 DOM에 추가 `appendChild()` 메소드를 사용해 생성된 노드를 DOM tree에 추가함. 또는 `removeChild()` 메소드를 사용해 DOM tree에서 노드를 삭제할 수 있음

**createElement(tagName)**

- 태그이름을 인자로 전달하여 요소를 생성함
- return: HTMLElement를 상속받은 객체
- 모든 브라우저에서 동작함

**createTextNode(text)**

- 텍스트를 인자로 전달하여 텍스트 노드를 생성함
- return: Text 객체
- 모든 브라우저에서 동작함

**appendChild(Node)**

- 인자로 전달한 노드를 마지막 자식 요소로 DOM 트리에 추가함
- return: 추가한 노드
- 모든 브라우저에서 동작함

**removeChild(Node)**

- 인자로 전달한 노드를 DOM 트리에 제거함
- return: 추가한 노드
- 모든 브라우저에서 동작함

```
// 태그이름을 인자로 전달하여 요소를 생성
const newElem = document.createElement('li');

// const newElem = document.createElement('<li>test</li>');
// Uncaught DOMException: Failed to execute 'createElement' on 'Document': The tag name provided ('<li>test</li>') is not a valid name.

// 텍스트 노드를 생성
const newText = document.createTextNode('Beijing');

// 텍스트 노드를 newElem 자식으로 DOM 트리에 추가
newElem.appendChild(newText);

const container = document.querySelector('ul');

// newElem을 container의 자식으로 DOM 트리에 추가. 마지막 요소로 추가된다.
container.appendChild(newElem);

const removeElem = document.getElementById('one');

// container의 자식인 removeElem 요소를 DOM 트리에 제거한다.
container.removeChild(removeElem);
```

### 4.5 insertAdjacentHTML()

**insertAdjacentHTML(position, string)**

- 인자로 전달한 텍스트를 HTML로 파싱하고 그 결과로 생성된 노드를 DOM 트리의 지정된 위치에 삽입함. 첫 번째 인자는 삽입 위치, 두 번째 인자는 삽입할 요소를 표현한 문자열임. 첫 번째 인자로 올 수 있는 값은 아래와 같음. `position`에 올 수 있는 값은 다음과 같음

  - beforebegin
  - afterbegin
  - beforeend
  - afterend

- 모든 브라우저에서 동작함

<div align="center">
<img src="https://poiemaweb.com/img/insertAdjacentHTML-position.png" width="50%" height="50%">
</div>

```
const one = document.getElementById('one');

// 마크업이 포함된 요소 추가
one.insertAdjacentHTML('beforeend', '<em class="blue">, Korea</em>');
```

### 4.6 innerHTML vs. DOM 조작 방식 vs. insertAdjacentHTML()

**innerHTML**

| 장점                                                      | 단점                                                                                                          |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| DOM 조작 방식에 비해 빠르고 간편함                        | XSS 공격에 취약점이 있기 때문에 사용자로부터 입력받은 콘텐츠(ex: 댓글, 사용자 이름 등)를 추가할 때 주의해야함 |
| 간편하게 문자열로 정의한 여러 요소를 DOM에 추가할 수 있음 | 해당 요소의 내용을 덮어씀. 즉, HTML을 다시 파싱함. 이것은 비효율적임                                          |
| 콘텐츠를 취득할 수 있음                                   |                                                                                                               |

**DOM 조작 방식**

| 장점                                   | 단점                                         |
| -------------------------------------- | -------------------------------------------- |
| 특정 노드 한 개를 DOM에 추가할 때 적합 | innerHTML보다는 느리고 더 많은 코드가 필요함 |

**insertAdjacentHTML**

| 장점                                                      | 단점                                                                                |
| --------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| 간편하게 문자열로 정의된 여러 요소를 DOM에 추가할 수 있음 | XSS 공격에 취약점이 있기 때문에 사용자로부터 입력받은 콘텐츠를 추가할 때 주의해야함 |
| 삽입되는 위치를 선정할 수 있음                            |                                                                                     |

**결론**

innerHTML과 insertAdjacentHTML은 크로스 스크립팅 공격에 취약함. 따라서 untrusted data의 경우 주의해야함. 텍스트를 추가 또는 변경시에는 textContent, 새로운 요소의 추가 또는 삭제시에는 DOM 조작 방식을 사용하도록 하자

## 5. style

style 프로퍼티를 사용하면 inline 스타일 선언을 생성함. 특정 요소에 inline 스타일을 지정하는 경우 사용함

```
const four = document.getElementById('four');

// inline 스타일 선언을 생성
four.style.color = 'blue';

// font-size와 같이 '-'으로 구분되는 프로퍼티는 카멜케이스로 변환하여 사용함
four.style.fontSize = '2em';
```

style 프로퍼티의 값을 취득하려면 `window.getComputedStyle`을 사용함. window.getComputedStyle 메소드는 인자로 주어진 요소의 모든 css 프로퍼티 값을 반환함

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>style 프로퍼티 값 취득</title>
  <style>
    .box {
      width: 100px;
      height: 50px;
      background-color: red;
      border: 1px solid black;
    }
  </style>
</head>
<body>
  <div class="box"></div>
  <script>
    const box = document.querySelector('.box');

    const width = getStyle(box, 'width');
    const height = getStyle(box, 'height');
    const backgroundColor = getStyle(box, 'background-color');
    const border = getStyle(box, 'border');

    console.log('width: ' + width);
    console.log('height: ' + height);
    console.log('backgroundColor: ' + backgroundColor);
    console.log('border: ' + border);

    /**
     * 요소에 적용된 CSS 프로퍼티를 반환
     * @param {HTTPElement} elem - 대상 요소 노드.
     * @param {string} prop - 대상 CSS 프로퍼티.
     * @returns {string} CSS 프로퍼티의 값.
     */
    function getStyle(elem, prop) {
      return window.getComputedStyle(elem, null).getPropertyValue(prop);
    }
  </script>
</body>
</html>
```
