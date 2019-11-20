
�ڼ��� ������ �Ʒ����� Ȯ���� �� �ֽ��ϴ�.

```js run
async function loadJson(url) { // (1)
  let response = await fetch(url); // (2)

  if (response.status == 200) {
    let json = await response.json(); // (3)
    return json;
  }

  throw new Error(response.status);
}

loadJson('no-such-user.json')
  .catch(alert); // Error: 404 (4)
```

����:

1. �Լ� `loadJson`�� `async` �Լ��� �˴ϴ�.
2. �Լ� ���� `.then`�� ���� `await`�� �ٲߴϴ�.
3. �� ���ó�� `await`�� ����ص� ������, �Ʒ�ó�� `return response.json()`�� ����ص� �˴ϴ�.

    ```js
    if (response.status == 200) {
      return response.json(); // (3)
    }
    ```

    ���, �̷��� �ۼ��ϸ� ����̽��� ����Ǵ°� `await`�� ����� �ٱ� �ڵ忡�� ��ٷ��� �մϴ�. �� ���ô� �ش� ������ ������ ������.
4. `loadJson`���� ������ ������ `.catch`���� ó���˴ϴ�. `loadJson`�� ȣ���ϴ� �ڵ�� `async` �Լ� ���ΰ� �ƴϱ� ������ `await loadJson(��)`�� ����� �� �����ϴ�.