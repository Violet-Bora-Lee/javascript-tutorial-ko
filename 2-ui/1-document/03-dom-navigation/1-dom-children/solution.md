����� �پ��մϴ�.


`<div>` DOM ���:

```js
document.body.firstElementChild
// �Ǵ�
document.body.children[0]
// �Ǵ� (ù ��° ���� �����̹Ƿ� �� ��° ��带 ������)
document.body.childNodes[1]
```

`<ul>` DOM ���:

```js
document.body.lastElementChild
// �Ǵ�
document.body.children[1]
```

�� ��° `<li>` (Pete):

```js
// <ul>�� ��������, <ul>�� ������ �ڽ� ��Ҹ� ������
document.body.lastElementChild.lastElementChild
```
