importance: 5

---

# "prototype" �����ϱ�

�Ʒ� �ڵ忡�� `new Rabbit`�� ����� `Rabbit`�� `"prototype"`�� �����մϴ�.

���� �ڵ�� ������ �����ϴ�.

```js run
function Rabbit() {}
Rabbit.prototype = {
  eats: true
};

let rabbit = new Rabbit();

alert( rabbit.eats ); // true
```


1. �Ʒ��� ���� �ڵ带 �߰�(������ ��)�ϸ� ��â�� ������ ��µɱ��?

    ```js
    function Rabbit() {}
    Rabbit.prototype = {
      eats: true
    };

    let rabbit = new Rabbit();

    *!*
    Rabbit.prototype = {};
    */!*

    alert( rabbit.eats ); // ?
    ```

2. �Ʒ��� ���� �ڵ带 �����ϸ� ��â�� ������ ��µɱ��?

    ```js
    function Rabbit() {}
    Rabbit.prototype = {
      eats: true
    };

    let rabbit = new Rabbit();

    *!*
    Rabbit.prototype.eats = false;
    */!*

    alert( rabbit.eats ); // ?
    ```

3. �Ʒ��� ���� `delete`�� ����ϸ� ��â�� ������ ��µɱ��?

    ```js
    function Rabbit() {}
    Rabbit.prototype = {
      eats: true
    };

    let rabbit = new Rabbit();

    *!*
    delete rabbit.eats;
    */!*

    alert( rabbit.eats ); // ?
    ```

4. ������ �ڵ带 �����ϸ� ��â�� ������ ��µɱ��?

    ```js
    function Rabbit() {}
    Rabbit.prototype = {
      eats: true
    };

    let rabbit = new Rabbit();

    *!*
    delete Rabbit.prototype.eats;
    */!*

    alert( rabbit.eats ); // ?
    ```