# 노드 프로퍼티: 타입, 태그 그리고 내용(type, tag and contents)

DOM 노드에 대하여 좀 더 살펴보도록 합시다.

이번 주제에선 노드가 무엇인지와 자주 쓰이는 노드 프로퍼티(property)에 대해서 알아보도록 하겠습니다.

## DOM 노드 클래스

DOM 노드는 클래스에 따라 각각 다른 프로퍼티를 가집니다. `<a>`태그에 대응하는 요소(element) 노드의 경우 링크와 관련된 프로퍼티를 가지고 `<input>`태그에 대응하는 요소 노드의 경우 입력과 관련된 프로퍼티를 가지는 것이 그 예입니다. 텍스트(text) 노드는 요소 노드와는 다릅니다. 하지만 모든 노드 클래스는 공통의 계층(Node)으로부터 상속되므로 같은 기본 프로퍼티와 메서드를 공유합니다.  

각각의 DOM 노드는 그에 대응하는 내장 클래스에 속합니다.

계층구조의 꼭대기엔 [EventTarget](https://dom.spec.whatwg.org/#eventtarget)이 있고, [Node](http://dom.spec.whatwg.org/#interface-node)는 이 EventTarget을 상속받습니다. 다른 DOM 노드들은 Node를 상속받습니다.

아래 그림은 이런 계층구조를 잘 나타냅니다.

![](dom-class-hierarchy.svg)

노드 클래스:

- [EventTarget](https://dom.spec.whatwg.org/#eventtarget) -- 계층의 뿌리에 있는 "추상" 클래스입니다. 이 클래스를 구현한 객체는 절대 생성되지 않습니다. 모든 노드가 "events"라는 걸 지원하도록 하는 기반의 역할을 합니다. 이 부분에 대해선 추후 이야기할 예정입니다.
- [Node](http://dom.spec.whatwg.org/#interface-node) -- DOM 노드의 기초가 되는 "추상" 클래스입니다. `parentNode`, `nextSibling`, `childNodes`와 같은 노드사이 이동에 관련된 getter 메서드를 지원합니다. `Node` 클래스를 구현한 객체는 절대 생성되지 않습니다. 하지만 이 추상클래스를 상속받는 구체적인 클래스는 존재합니다. 텍스트 노드를 위한 `Text`, 요소, 요소(Element) 노드를 위한 `Element`, Comment 노드를 위한 `Comment`가 그 예입니다.
- [Element](http://dom.spec.whatwg.org/#interface-element) -- DOM 요소의 기초가 되는 클래스입니다. `nextElementSibling`, `children`와 같은 요소 레벨 탐색 관련 프로퍼티와 `getElementsByTagName`, `querySelector`와 같은 검색 메서드를 제공합니다. 브라우저에는 HTML뿐만 아니라 XML, SVG가 있을 수 있습니다. `Element` 클래스는 `SVGElement`, `XMLElement`, `HTMLElement`의 기초가 됩니다.
- [HTMLElement](https://html.spec.whatwg.org/multipage/dom.html#htmlelement) -- 모든 HTML 요소의 기초가 되는 클래스입니다. 다양한 HTML 요소들은 이 클래스를 상속받습니다.
    - [HTMLInputElement](https://html.spec.whatwg.org/multipage/forms.html#htmlinputelement) -- `<input>` 요소를 위한 클래스
    - [HTMLBodyElement](https://html.spec.whatwg.org/multipage/semantics.html#htmlbodyelement) -- `<body>` 요소를 위한 클래스
    - [HTMLAnchorElement](https://html.spec.whatwg.org/multipage/semantics.html#htmlanchorelement) -- `<a>` 요소를 위한 클래스
    - 이 외에도 각각의 태그는 특정 프로퍼티와 메서드를 제공하는 자신만의 클래스를 가집니다.

각 노드의 프로퍼티와 메서드는 상속으로부터 만들어집니다.

예를 들어 `<input>` 요소에 대응하는 DOM 객체를 생각해 봅시다. 이 객체는 [HTMLInputElement](https://html.spec.whatwg.org/multipage/forms.html#htmlinputelement) 클래스에 속합니다. 따라서 아래 상속 관계에 따라 프로퍼티와 메서드를 갖게 됩니다:

- `HTMLInputElement` -- 이 클래스는 입력과 관련된 프로퍼티를 제공합니다. 그리고 아래(`HTMLElement`)로부터 상속됩니다.
- `HTMLElement` -- HTML 요소 메서드(getter/setter)를 제공합니다. 그리고 아래로부터 상속됩니다.
- `Element` -- 전반적인 요소 메서드를 제공합니다. 그리고 아래로부터 상속됩니다.
- `Node` -- 전반적인 DOM 노드 프로퍼티를 제공합니다. 그리고 아래로부터 상속됩니다.
- `EventTarget` -- 이벤트와 관련된 일을 합니다(뒤에서 다룰 예정입니다).
- ... `EventTarget`은 `Object`를 상속받습니다 따라서 `hasOwnProperty`와 같은 순수 객체 관련 메서드를 사용할 수 있습니다.

DOM 노드 클래스 이름을 확인하려면 객체가 `constructor` 프로퍼티를 가진다는 점을 이용할 수 있습니다. `constructor` 프로퍼티는 클래스 생성자를 참조하고, `constructor.name`을 통해 이름을 알아낼 수 있습니다.

```js run
alert( document.body.constructor.name ); // HTMLBodyElement
```

...`toString`을 사용해도 됩니다:

```js run
alert( document.body ); // [object HTMLBodyElement]
```

`instanceof` 를 사용해 상속 관계를 확인할 수 있습니다:

```js run
alert( document.body instanceof HTMLBodyElement ); // true
alert( document.body instanceof HTMLElement ); // true
alert( document.body instanceof Element ); // true
alert( document.body instanceof Node ); // true
alert( document.body instanceof EventTarget ); // true
```

위에서 확인한 바와 같이 DOM 노드도 자바스크립트 객체이므로, 프로토타입 기반의 상속 관계를 가집니다.

브라우저 콘솔에 `console.dir(elem)`를 입력하면 이런 관계를 눈으로 볼 수 있습니다. `HTMLElement.prototype`, `Element.prototype`등이 콘솔에 출력될 것입니다.

```smart header="`console.dir(elem)` 와 `console.log(elem)`"
브라우저 대부분은 개발자 도구에서 `console.log` 와 `console.dir` 명령어를 지원합니다. 이 명령어들은 콘솔에 인수(argument)를 출력해줍니다. 자바스크립트 객체에선 이 두 명령어가 각은 역할을 합니다.

하지만 DOM 요소에선 다른 출력값을 보입니다:

- `console.log(elem)` 는 요소(elem)의 DOM 트리를 출력하고,
- `console.dir(elem)` 는 요소(elem)를 DOM 객체처럼 취급하여 출력합니다. 따라서 프로퍼티를 확인하기 쉽다는 장점이 있습니다.

`document.body`를 통해 그 차이를 직접 확인해보세요.
```

````smart header="스펙 문서에서 쓰이는 IDL"
스펙 문서에선 DOM 클래스를 JavaScript가 아닌 이해하기 쉬운 표기법인 [Interface description language](https://en.wikipedia.org/wiki/Interface_description_language) (IDL)을 이용하여 설명합니다.

IDL은 모든 프로퍼티의 앞에 타입을 붙여서 작성됩니다. `DOMString`과 `boolean` 과 같은 타입이 프로퍼티 앞에 붙게 됩니다.

아래는 스펙 문서의 일부를 발췌하여 주석을 달아놓은 예시입니다:

```js
// HTMLInputElement를 정의
*!*
// 콜론 ":" 은 HTMLInputElement가 HTMLElement로 부터 상속되었다는 것을 의미함 
*/!*
interface HTMLInputElement: HTMLElement {
  // <input> elements 와 관련된 프로퍼티와 메서드

*!*
  // "DOMString"은 아래 프로퍼티들이 문자열이라는 것을 의미함
*/!*
  attribute DOMString accept;
  attribute DOMString alt;
  attribute DOMString autocomplete;
  attribute DOMString value;

*!*
  // 불린값을 가지는 프로퍼티 (true/false)
  attribute boolean autofocus;
*/!*
  ...
*!*
  // "void"는 해당 메서드의 리턴값이 없음을 의미함
*/!*
  void select();
  ...
}
```

다른 클래스에서도 유사한 문법을 확인할 수 있습니다.
````

## nodeType 프로퍼티

`nodeType` 프로퍼티는 DOM 노드의 "타입"을 알아내고자 할 때 쓰이는 옛날식의 프로퍼티입니다.

노드 타입은 상숫값을 가집니다:
- `elem.nodeType == 1` 은 요소 노드,
- `elem.nodeType == 3` 은 텍스트(Text) 노드,
- `elem.nodeType == 9` 은 DOCUMENT 객체(document object)를 나타내고,
- [스펙 문서](https://dom.spec.whatwg.org/#node)로 가면 다양한 노드 타입을 볼 수 있습니다.

예시:

```html run
<body>
  <script>  
  let elem = document.body;

  // 어떤 타입일까요?
  alert(elem.nodeType); // 1 => 요소

  // 첫번째 자식 노드는...
  alert(elem.firstChild.nodeType); // 3 => 텍스트

  // 문서객체는 타입이 9
  alert( document.nodeType ); // 9
  </script>
</body>
```

모던 자바스크립트에선 노드 타입을 `instanceof`나 다른 클래스 기반의 테스트를 이용해 확인합니다. 하지만 가끔은 `nodeType`를 쓰는 게 간단할 때도 있습니다. `nodeType`을 쓰면 오직 타입을 읽기만 하고 바꾸지는 못합니다.

## 태그: nodeName 과 tagName

`nodeName` 이나 `tagName` 프로퍼티를 사용하면 DOM 노드의 태그 이름을 알아낼 수 있습니다.

예:

```js run
alert( document.body.nodeName ); // BODY
alert( document.body.tagName ); // BODY
```

그럼 `tagName` 과 `nodeName` 차이는 없는걸까요?

물론 있습니다. 미묘하지만 이름에서 그 차이를 확인할 수 있습니다.

- `tagName` 프로퍼티는 `요소(Element)` 노드에만 존재합니다.
- `nodeName`은 모든 `Node`에서 찾을 수 있습니다:
    - 요소노드에선 `tagName`과 같은 역할을 합니다.
    - text, comment 등의 노드 타입에선 노드 타입을 나타내는 문자열을 리턴합니다.

`nodeName`은 모든 노드에서 지원되지만, `tagName`은 `Element` 클래스로부터 유래되었기 때문에 요소 노드에서만 지원됩니다. 

`document`와 comment 노드로 `tagName` 과 `nodeName`의 차이점을 확인 해 봅시다:


```html run
<body><!-- comment -->

  <script>
    // for comment
    alert( document.body.firstChild.tagName ); // undefined (요소가 아님)
    alert( document.body.firstChild.nodeName ); // #comment

    // for document
    alert( document.tagName ); // undefined (요소가 아님)
    alert( document.nodeName ); // #document
  </script>
</body>
```

요소를 다루고 있다면 `tagName`을 사용하면 됩니다.


```smart header="태그 이름은 XHTML을 제외하고 항상 대문자입니다"
브라우저는 HTML과 XML유형의 문서를 다른 방법으로 처리합니다. 웹페이지는 대게 HTML 모드로 처리됩니다. 헤더가 `Content-Type: application/xml+xhtml`인 XML-문서의 경우는 XML 모드로 처리됩니다.

HTML 모드에선 `tagName/nodeName`이 모두 대문자를 리턴합니다. `<body>` 이든 `<BoDy>` 상관없이 `BODY`를 리턴합니다.

XML 모드에선 문자가 그대로 유지되는데, 최근엔 거의 사용되지 않습니다.
```


## innerHTML: 내용 들여다보기

[innerHTML](https://w3c.github.io/DOM-Parsing/#widl-Element-innerHTML) 프로퍼티를 사용하면 요소 안의 HTML을 문자열로 받아올 수 있습니다.

요소 안의 HTML을 수정하는 것도 가능합니다. innerHTML은 페이지를 수정하는 데 쓰이는 강력한 방법의 하나입니다.

아래는 `document.body`안의 내용(contents)을 출력하고 완전히 바꾸는 예시입니다:

```html run
<body>
  <p>A paragraph</p>
  <div>A div</div>

  <script>
    alert( document.body.innerHTML ); // 현재 내용 읽기
    document.body.innerHTML = 'The new BODY!'; // 내용 교체하기
  </script>

</body>
```

문법이 틀린 HTML을 넣게 되면 브라우저가 자동으로 이를 고쳐 줄 때도 있습니다:

```html run
<body>

  <script>
    document.body.innerHTML = '<b>test'; // 닫는 태그를 넣지 않음
    alert( document.body.innerHTML ); // <b>test</b> (수정됨)
  </script>

</body>
```

```smart header="Scripts don't execute"
If `innerHTML` inserts a `<script>` tag into the document -- it becomes a part of HTML, but doesn't execute.
```

### Beware: "innerHTML+=" does a full overwrite

We can append HTML to an element by using `elem.innerHTML+="more html"`.

Like this:

```js
chatDiv.innerHTML += "<div>Hello<img src='smile.gif'/> !</div>";
chatDiv.innerHTML += "How goes?";
```

But we should be very careful about doing it, because what's going on is *not* an addition, but a full overwrite.

Technically, these two lines do the same:

```js
elem.innerHTML += "...";
// is a shorter way to write:
*!*
elem.innerHTML = elem.innerHTML + "..."
*/!*
```

In other words, `innerHTML+=` does this:

1. The old contents is removed.
2. The new `innerHTML` is written instead (a concatenation of the old and the new one).

**As the content is "zeroed-out" and rewritten from the scratch, all images and other resources will be reloaded**.

In the `chatDiv` example above the line `chatDiv.innerHTML+="How goes?"` re-creates the HTML content and reloads `smile.gif` (hope it's cached). If `chatDiv` has a lot of other text and images, then the reload becomes clearly visible.

There are other side-effects as well. For instance, if the existing text was selected with the mouse, then most browsers will remove the selection upon rewriting `innerHTML`. And if there was an `<input>` with a text entered by the visitor, then the text will be removed. And so on.

Luckily, there are other ways to add HTML besides `innerHTML`, and we'll study them soon.

## outerHTML: full HTML of the element

The `outerHTML` property contains the full HTML of the element. That's like `innerHTML` plus the element itself.

Here's an example:

```html run
<div id="elem">Hello <b>World</b></div>

<script>
  alert(elem.outerHTML); // <div id="elem">Hello <b>World</b></div>
</script>
```

**Beware: unlike `innerHTML`, writing to `outerHTML` does not change the element. Instead, it replaces it as a whole in the outer context.**

Yeah, sounds strange, and strange it is, that's why we make a separate note about it here. Take a look.

Consider the example:

```html run
<div>Hello, world!</div>

<script>
  let div = document.querySelector('div');

*!*
  // replace div.outerHTML with <p>...</p>
*/!*
  div.outerHTML = '<p>A new element!</p>'; // (*)

*!*
  // Wow! The div is still the same!
*/!*
  alert(div.outerHTML); // <div>Hello, world!</div>
</script>
```

In the line `(*)` we take the full HTML of `<div>...</div>` and replace it by `<p>...</p>`. In the outer document we can see the new content instead of the `<div>`. But the old `div` variable is still the same.

The `outerHTML` assignment does not modify the DOM element, but extracts it from the outer context and inserts a new piece of HTML instead of it.

Novice developers sometimes make an error here: they modify `div.outerHTML` and then continue to work with `div` as if it had the new content in it.

That's possible with `innerHTML`, but not with `outerHTML`.

We can write to `outerHTML`, but should keep in mind that it doesn't change the element we're writing to. It creates the new content on its place instead. We can get a reference to new elements by querying DOM.

## nodeValue/data: text node content

The `innerHTML` property is only valid for element nodes.

Other node types have their counterpart: `nodeValue` and `data` properties. These two are almost the same for practical use, there are only minor specification differences. So we'll use `data`, because it's shorter.

An example of reading the content of a text node and a comment:

```html run height="50"
<body>
  Hello
  <!-- Comment -->
  <script>
    let text = document.body.firstChild;
*!*
    alert(text.data); // Hello
*/!*

    let comment = text.nextSibling;
*!*
    alert(comment.data); // Comment
*/!*
  </script>
</body>
```

For text nodes we can imagine a reason to read or modify them, but why comments? Usually, they are not interesting at all, but sometimes developers embed information or template instructions into HTML in them, like this:

```html
<!-- if isAdmin -->
  <div>Welcome, Admin!</div>
<!-- /if -->
```

...Then JavaScript can read it and process embedded instructions.

## textContent: pure text

The `textContent` provides access to the *text* inside the element: only text, minus all `<tags>`.

For instance:

```html run
<div id="news">
  <h1>Headline!</h1>
  <p>Martians attack people!</p>
</div>

<script>
  // Headline! Martians attack people!
  alert(news.textContent);
</script>
```

As we can see, only text is returned, as if all `<tags>` were cut out, but the text in them remained.

In practice, reading such text is rarely needed.

**Writing to `textContent` is much more useful, because it allows to write text the "safe way".**

Let's say we have an arbitrary string, for instance entered by a user, and want to show it.

- With `innerHTML` we'll have it inserted "as HTML", with all HTML tags.
- With `textContent` we'll have it inserted "as text", all symbols are treated literally.

Compare the two:

```html run
<div id="elem1"></div>
<div id="elem2"></div>

<script>
  let name = prompt("What's your name?", "<b>Winnie-the-pooh!</b>");

  elem1.innerHTML = name;
  elem2.textContent = name;
</script>
```

1. The first `<div>` gets the name "as HTML": all tags become tags, so we see the bold name.
2. The second `<div>` gets the name "as text", so we literally see `<b>Winnie-the-pooh!</b>`.

In most cases, we expect the text from a user, and want to treat it as text. We don't want unexpected HTML in our site. An assignment to `textContent` does exactly that.

## The "hidden" property

The "hidden" attribute and the DOM property specifies whether the element is visible or not.

We can use it in HTML or assign using JavaScript, like this:

```html run height="80"
<div>Both divs below are hidden</div>

<div hidden>With the attribute "hidden"</div>

<div id="elem">JavaScript assigned the property "hidden"</div>

<script>
  elem.hidden = true;
</script>
```

Technically, `hidden` works the same as `style="display:none"`. But it's shorter to write.

Here's a blinking element:


```html run height=50
<div id="elem">A blinking element</div>

<script>
  setInterval(() => elem.hidden = !elem.hidden, 1000);
</script>
```

## More properties

DOM elements also have additional properties, many of them provided by the class:

- `value` -- the value for `<input>`, `<select>` and `<textarea>` (`HTMLInputElement`, `HTMLSelectElement`...).
- `href` -- the "href" for `<a href="...">` (`HTMLAnchorElement`).
- `id` -- the value of "id" attribute, for all elements (`HTMLElement`).
- ...and much more...

For instance:

```html run height="80"
<input type="text" id="elem" value="value">

<script>
  alert(elem.type); // "text"
  alert(elem.id); // "elem"
  alert(elem.value); // value
</script>
```

Most standard HTML attributes have the corresponding DOM property, and we can access it like that.

If we want to know the full list of supported properties for a given class, we can find them in the specification. For instance, HTMLInputElement is documented at <https://html.spec.whatwg.org/#htmlinputelement>.

Or if we'd like to get them fast or are interested in a concrete browser specification -- we can always output the element using `console.dir(elem)` and read the properties. Or explore "DOM properties" in the Elements tab of the browser developer tools.

## Summary

Each DOM node belongs to a certain class. The classes form a hierarchy. The full set of properties and methods come as the result of inheritance.

Main DOM node properties are:

`nodeType`
: We can use it to see if a node is a text or an element node. It has a numeric value: `1` -- for elements,`3` -- for text nodes, and few other for other node types. Read-only.

`nodeName/tagName`
: For elements, tag name (uppercased unless XML-mode). For non-element nodes `nodeName` describes what it is. Read-only.

`innerHTML`
: The HTML content of the element. Can be modified.

`outerHTML`
: The full HTML of the element. A write operation into `elem.outerHTML` does not touch `elem` itself. Instead it gets replaced with the new HTML in the outer context.

`nodeValue/data`
: The content of a non-element node (text, comment). These two are almost the same, usually we use `data`. Can be modified.

`textContent`
: The text inside the element: HTML minus all `<tags>`. Writing into it puts the text inside the element, with all special characters and tags treated exactly as text. Can safely insert user-generated text and protect from unwanted HTML insertions.

`hidden`
: When set to `true`, does the same as CSS `display:none`.

정리하자면 `innerHTML+=` 은 다음과 같은 기능을 수행합니다:

Although, HTML attributes and DOM properties are not always the same, as we'll see in the next chapter.
