libs:
  - d3
  - domtree

---


# Walking the DOM

The DOM allows us to do anything with elements and their contents, but first we need to reach the corresponding DOM object, get it into a variable, and then we are able to modify it.

All operations on the DOM start with the `document` object. From it we can access any node.

Here's a picture of links that allow for travel between DOM nodes:

![](dom-links.png)

Let's discuss them in more detail.

## On top: documentElement and body

The topmost tree nodes are available directly as `document` properties:

`<html>` = `document.documentElement`
: The topmost document node is `document.documentElement`. That's DOM node of `<html>` tag.

`<body>` = `document.body`
: Another widely used DOM node is the `<body>` element -- `document.body`.

`<head>` = `document.head`
: The `<head>` tag is available as `document.head`.

````warn header="There's a catch: `document.body` can be `null`"
A script cannot access an element that doesn't exist at the moment of running.

In particular, if a script is inside `<head>`, then `document.body` is unavailable, because the browser did not read it yet.

So, in the example below the first `alert` shows `null`:

```html run
<html>

<head>
  <script>
*!*
    alert( "From HEAD: " + document.body ); // null, there's no <body> yet
*/!*
  </script>
</head>

<body>

  <script>
    alert( "From BODY: " + document.body ); // HTMLBodyElement, now it exists
  </script>

</body>
</html>
```
````

```smart header="In the DOM world `null` means \"doesn't exist\""
In the DOM, the `null` value means "doesn't exist" or "no such node".
```

## Children: childNodes, firstChild, lastChild

There are two terms that we'll use from now on:

- **Child nodes (or children)** -- elements that are direct children. In other words, they are nested exactly in the given one. For instance, `<head>` and `<body>` are children of `<html>` element.
- **Descendants** -- all elements that are nested in the given one, including children, their children and so on.

For instance, here `<body>` has children `<div>` and `<ul>` (and few blank text nodes):

```html run
<html>
<body>
  <div>Begin</div>

  <ul>
    <li>
      <b>Information</b>
    </li>
  </ul>
</body>
</html>
```

...And if we ask for all descendants of `<body>`, then we get direct children `<div>`, `<ul>` and also more nested elements like `<li>` (being a child of `<ul>`) and `<b>` (being a child of `<li>`) -- the entire subtree.

**The `childNodes` collection provides access to all child nodes, including text nodes.**

The example below shows children of `document.body`:

```html run
<html>
<body>
  <div>Begin</div>

  <ul>
    <li>Information</li>
  </ul>

  <div>End</div>

  <script>
*!*
    for (let i = 0; i < document.body.childNodes.length; i++) {
      alert( document.body.childNodes[i] ); // Text, DIV, Text, UL, ..., SCRIPT
    }
*/!*
  </script>
  ...more stuff...
</body>
</html>
```

Please note an interesting detail here. If we run the example above, the last element shown is `<script>`. In fact, the document has more stuff below, but at the moment of the script execution the browser did not read it yet, so the script doesn't see it.

**Properties `firstChild` and `lastChild` give fast access to the first and last children.**

They are just shorthands. If there exist child nodes, then the following is always true:
```js
elem.childNodes[0] === elem.firstChild
elem.childNodes[elem.childNodes.length - 1] === elem.lastChild
```

There's also a special function `elem.hasChildNodes()` to check whether there are any child nodes.

### DOM 컬렉션

위에서 살펴본 `childNodes`는 마치 배열 같아 보입니다. 하지만 `childNodes`는 배열이 아닌 *컬렉션(collection)*입니다. iterable(반복 가능한) 유사 배열 객체이죠. 

`childNodes`는 컬렉션이기 때문에 아래와 같은 특징을 가집니다.

1. `for..of`를 사용할 수 있습니다.
  ```js
  for (let node of document.body.childNodes) {
    alert(node); // 컬렉션 내의 모든 노드를 보여줍니다
  }
  ```
  That's because it's iterable (provides the `Symbol.iterator` property, as required).

2. 배열이 아니기 때문에 배열 메서드를 쓸 수 없습니다.
  ```js run
  alert(document.body.childNodes.filter); // undefined (filter 메서드가 없습니다)
  ```

첫 번째 특징은 유용합니다. 두 번째 특징도 썩 좋지는 않지만 참을 만합니다. `Array.from`을 사용하면 컬렉션을 가지고 "진짜" 배열을 만들 수 있기 때문입니다. 배열 메서드를 쓰고 싶다면, 이를 이용하면 됩니다. 

  ```js run
  alert( Array.from(document.body.childNodes).filter ); // 이제 filter메서드를 사용할 수 있습니다. 
  ```

```warn header="DOM 컬렉션은 읽는 것만 가능합니다."
DOM 컬렉션을 비롯해 이번 챕터에서 설명하는 *모든* 탐색용 프로퍼티는 읽기 전용입니다.  

childNodes[i] = ...`를 이용해 자식 노드를 교체하 는게 불가능합니다.

DOM을 변경하려면 다른 메서드가 필요합니다. 다음 챕터에서 이 메서드에 대해 살펴보겠습니다.
```

```warn header="DOM 컬렉션은 살아있습니다."
몇몇 예외사항을 제외하고 거의 모든 DOM 컬렉션은 *살아있습니다*. DOM의 현재 상태를 반영한다는 말이죠.

`elem.childNodes`를 이용해 컬렉션을 참조하고 있는 도중에 DOM에 새로운 노드가 더해지거나 삭제되면, 이 변경사항이 컬렉션에도 자동으로 반영됩니다.
```

````warn header="컬렉션에 `for..in` 반복문을 사용하지 마세요"
컬렉션은 `for..of`를 이용해 반복가능합니다. 그런데 가끔 `for..in`을 사용하려는 사람들이 있습니다.

`for..in`은 절대 사용하지 마세요. `for..in`반복문은 객체의 모든 열거 가능한 프로퍼티에 반복을 시행합니다. 컬렉션엔 거의 사용되지 않는 "추가" 프로퍼티가 있는데, 이 프로퍼티에 까지 반복문이 돌길 원하지 않으실 거니까요.

```html run
<body>
<script>
  // shows 0, 1, length, item, values and more.
  for (let prop in document.body.childNodes) alert(prop);
</script>
</body>
````

## 형제와 부모 노드

*형제 노드*는 같은 부모를 가진 노드 사이의 관계를 의미합니다. `<head>`와 `<body>`가 대표적인 형제 관계의 노드입니다. 

- `<body>`는 `<head>`의 "다음(next)" 또는 "우측(right)" 형제 노드입니다.
- `<head>`는 `<body>`의 "이전(previous)" 또는 "좌측(left)" 형제 노드입니다.

`parentNode`를 이용하면 부모 노드를 참조할 수 있습니다.

같은 부모를 가진 다음 노드(형제 노드)로는 `nextSibling`, 이전 노드로는 `previousSibling`을 이용하여 이동할 수 있습니다.

예시:

```html run
<html><head></head><body><script>
  // "빈" text 노드를 없애려고 HTML을 "압축"하였습니다.

  // <body>의 부모는 <html>입니다
  alert( document.body.parentNode === document.documentElement ); // true

  // <head> 다음 노드는 <body>입니다.
  alert( document.head.nextSibling ); // HTMLBodyElement

  // <body> 이전 노드는 <head>입니다.
  alert( document.body.previousSibling ); // HTMLHeadElement
</script></body></html>
```

## 요소 간 이동

위에서 언급된 탐색에 관한 프로퍼티는 *모든* 노드를 참조합니다. `childNodes`를 이용하면 텍스트 노드, 요소 노드, 심지어 주석 노드까지 참조할 수 있습니다. 물론 노드가 존재하면 말이죠.

하지만 실무에서 텍스트 노드나 주석 노드는 잘 다루지 않습니다. 웹 페이지를 구성하는 태그의 분신인 요소 노드를 조작해야 하는 작업이 대다수이죠.  

이런 점을 반영하여, 지금부턴 *요소 노드*만을 고려하는 탐색 관계를 살펴보도록 하겠습니다.

![](dom-links-elements.png)

그림 속 관계는 위쪽에서 다뤘던 모든 종류의 노드 간 관계와 유사해 보입니다. `요소(Element)`라는 단어가 추가된 점만 다르네요.

- `children` 프로퍼티는 해당 요소의 자식 요소 노드를 가리킵니다.
- `firstElementChild`와 `lastElementChild` 프로퍼티는 각각 첫 번째 자식 요소와 마지막 자식 요소를 가리킵니다.
- `previousElementSibling`와 `nextElementSibling`는 형제 요소를 가리킵니다.
- `parentElement` 는 부모 요소를 가리킵니다.

````smart header="부모가 요소라는 보장이 *없는데* 왜 `parentElement`를 쓰나요?"
`parentElement` 프로퍼티는 부모 "요소(노드)"를 반환합니다. 반면, `parentNode` 프로퍼티는 "모든 종류의 부모 노드"를 반환하죠. 부모를 참조한다는 점에서 두 프로퍼티는 대게 같습니다.

하지만 한가지 예외가 있는데, `document.documentElement`를 다룰 때입니다.

```js run
alert( document.documentElement.parentNode ); // document
alert( document.documentElement.parentElement ); // null
```

`documentElement`프로퍼티는 HTML 페이지의 루트 노드인 `<html>` 요소 노드를 가리킵니다. 이 루트 노드는 `document` 노드를 부모로 가집니다. 그런데 `document` 노드는 요소 노드가 아니기 때문에, `documentElement`의 `parentNode`는 `document` 노드를 가리키지만, `documentElement`의 `parentElement`는 `document` 노드를 가리키지 않습니다. 부모 노드를 돌면서 각 노드에 있는 특정 메서드를 호출할 때 위와 같은 차이점을 고려 해야 할 때가 생길 겁니다. `document`노드엔 이 메서드가 없기 때문에, 메서드가 제외됩니다.
```

`childNodes`를 `children`으로 대체해, 위에서 쓰였던 예제 중 하나를 다시 작성해 보도록 하겠습니다. 이젠 오직 요소 노드만 출력되네요.

```html run
<html>
<body>
  <div>Begin</div>

  <ul>
    <li>Information</li>
  </ul>

  <div>End</div>

  <script>
*!*
    for (let elem of document.body.children) {
      alert(elem); // DIV, UL, DIV, SCRIPT
    }
*/!*
  </script>
  ...
</body>
</html>
```

## 테이블 탐색하기 [#dom-navigation-tables]

지금까진 탐색에 쓰이는 기본적인 프로퍼티를 알아보았습니다.

특정 타입의 DOM 요소는 기본 프로퍼티 외에 추가적인 프로퍼티를 지원합니다. 편의를 위해서이죠. 

테이블은 추가적인 프로퍼티를 지원하는 요소 중 하나입니다.

**`<table>`** 요소는 위에서 배운 기본 프로퍼티 이외에 아래의 프로퍼티도 지원합니다.
- `table.rows`는 테이블 내 `<tr>`요소를 담은 컬렉션을 가리킵니다.
- `table.caption/tHead/tFoot`은 각각 `<caption>`, `<thead>`, `<tfoot>` 요소를 가리킵니다.
- `table.tBodies`는 테이블 내 `<tbody>` 요소를 담은 컬렉션을 참조합니다(표준에 따르면, table 내에 여러 개의 tbody가 존재하는 게 가능합니다).

**`<thead>`, `<tfoot>`, `<tbody>`** 요소는 `rows` 프로퍼티를 지원합니다.
- `tbody.rows`는 tbody 내 `<tr>` 요소 컬렉션을 가리킵니다.

**`<tr>`:**
- `tr.cells`은 주어진 `<tr>` 안의 모든 `<td>`, `<th>`을 담은 컬렉션을 반환합니다.
- `tr.sectionRowIndex`는 주어진 `<tr>`이 `<thead>/<tbody>/<tfoot>`안쪽에서 몇 번째 줄에 위치하는지를 나타내는 인덱스(index)를 반환합니다.
- `tr.rowIndex`는 `<table>`내에서 해당 `<tr>`이 몇 번째 줄인지를 나타내는 인덱스를 반환합니다.

**`<td>`과 `<th>`:**
- `td.cellIndex`는 주어진 `<td>`가 `<tr>`내 몇 번째 위치해 있는지를 나타내는 인덱스를 반환합니다.

사용 예시:

```html run height=100
<table id="table">
  <tr>
    <td>one</td><td>two</td>
  </tr>
  <tr>
    <td>three</td><td>four</td>
  </tr>
</table>

<script>
  // 첫번째 줄, 두번째 칸의 내용을 출력
  alert( table.*!*rows[0].cells[1]*/!*.innerHTML ) // "two"
</script>
```

표에 관련한 공식 스펙은 [tabular data](https://html.spec.whatwg.org/multipage/tables.html)에서 찾아볼 수 있습니다.

테이블과 마찬가지로, HTML 폼(form)에만 쓸 수 있는 몇 가지 탐색 프로퍼티도 존재합니다. 폼을 배우면서 이 프로퍼티에 대해서도 살펴보도록 하겠습니다.

# 요약

탐색 프로퍼티(navigation property)를 사용하면, 특정 DOM 노드에서 이웃해있는 다른 노드로 바로 이동할 수 있습니다.

탐색 프로퍼티는 크게 두 개의 집합으로 나뉩니다.

- 모든 노드에 적용 가능한 `parentNode`, `childNodes`, `firstChild`, `lastChild`, `previousSibling`, `nextSibling`.
- 요소 노드에만 적용 가능한 `parentElement`, `children`, `firstElementChild`, `lastElementChild`, `previousElementSibling`, `nextElementSibling`.

테이블과 같은 몇몇 특정 타입의 DOM 요소는 콘텐츠를 읽을 수 있게 해주는 프로퍼티와 컬렉션을 제공하기도 합니다.