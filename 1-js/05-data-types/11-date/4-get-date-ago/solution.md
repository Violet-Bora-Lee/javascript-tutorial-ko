���� ���̵��� �����մϴ�. `date`���� �־��� ���ڸ� ���� �˴ϴ�.

```js
function getDateAgo(date, days) {
  date.setDate(date.getDate() - days);
  return date.getDate();
}
```

�׷��� ���ǻ��׿��� `date`�� �������� ����� �߱� ������ ���� ���� �ۼ��ϸ� ������ �˴ϴ�. �ܺ� �ڵ忡�� `date`�� ����ϰ� �ִ� ���, `date`�� �����ϰ� �Ǹ� ��ġ �ʴ� ���� �߻��� �� �ֽ��ϴ�. 

�Ʒ��� ���� `date`�� �����ϸ� `date`�� �����Ű�� �ʰ� ���ϴ� ����� ������ �� �ֽ��ϴ�.

```js run demo
function getDateAgo(date, days) {
  let dateCopy = new Date(date);

  dateCopy.setDate(date.getDate() - days);
  return dateCopy.getDate();
}

let date = new Date(2015, 0, 2); // 2015�� 1�� 2��

alert( getDateAgo(date, 1) ); // 1, (2015�� 1�� 1��)
alert( getDateAgo(date, 2) ); // 31, (2014�� 12�� 31��)
alert( getDateAgo(date, 365) ); // 2, (2014�� 1�� 2��)