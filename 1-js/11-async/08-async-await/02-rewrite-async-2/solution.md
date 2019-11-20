
`async/await`�� ���ο��� ��� �����ϴ��� �˾ƾ� ������ Ǯ �� �ֽ��ϴ�.

`async` �Լ��� ȣ���ϸ� ����̽��� ��ȯ�ǹǷ�, `.then`�� ���̸� �˴ϴ�.
```js run
async function wait() {
  await new Promise(resolve => setTimeout(resolve, 1000));

  return 10;
}

function f() {
  // shows 10 after 1 second
*!*
  wait().then(result => alert(result));
*/!*
}

f();
```