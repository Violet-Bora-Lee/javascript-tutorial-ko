`"constructor"` ������Ƽ�� ����� �� ���� ����Ǿ��ִٸ� ���� ���� ���ٹ��� �����մϴ�.

�⺻ `"prototype"`�� �������� �ʾҴٸ� �Ʒ� ���ô� �ǵ��� ��� �����մϴ�.

```js run
function User(name) {
  this.name = name;
};

let user = new User('John');
let user2 = new user.constructor('Pete');

alert( user2.name ); // Pete (�� �����ϳ׿�!)
```

`User.prototype.constructor == User`�̱� ������ �� ���ô� ����� �����մϴ�.

�׷��� �������� `User.prototype`�� ����� `User`�� �����ϴ� `constructor`�� �ٽ� ������ִ� �� �ؾ��ٸ� ������ ���ٹ��� �����մϴ�.

����:

```js run
function User(name) {
  this.name = name;
};
*!*
User.prototype = {}; // (*)
*/!*

let user = new User('John');
let user2 = new user.constructor('Pete');

alert( user2.name ); // undefined
```

�� `user2.name`�� `undefined`�� �ɱ��?

�� ������ `new user.constructor('Pete')`�� �Ʒ��� ���� �����ϱ� �����Դϴ�.

1. `new user.constructor('Pete')`�� `user`���� `constructor`�� ã�µ� �ƹ��͵� ã�� ���մϴ�.
2. ��ü���� ���ϴ� ������Ƽ�� ã�� ���߱� ������ ������Ÿ�Կ��� �˻��� �����մϴ�. `user`�� ������Ÿ���� `User.prototype`�ε�, `User.prototype`�� �� ��ü�Դϴ�.
3. `User.prototype`�� �Ϲ� ��ü `{}`�̰�, �Ϲ� ��ü�� ������Ÿ���� `Object.prototype`�Դϴ�. `Object.prototype.constructor == Object`�̹Ƿ� `Object`�� ���˴ϴ�.

�ᱹ�� `let user2 = new user.constructor('Pete');`�� `let user2 = new Object('Pete')`�� �˴ϴ�. �׷��� `Object`�� �����ڴ� �μ��� �����ϰ� �׻� �� ��ü�� �����մϴ�. ���� `let user2 = new Object('Pete')`�� `let user2 = {}`�� ���ٰ� ������ �� �ֽ��ϴ�. `user2.name`�� `undefined`�� ������ ���⿡ �ֽ��ϴ�.