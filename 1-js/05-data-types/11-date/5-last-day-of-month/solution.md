���� ���� ��Ÿ���� ��ü�� ����� `day`���� `0`�� �Ѱ��ָ� �˴ϴ�.
```js run demo
function getLastDayOfMonth(year, month) {
  let date = new Date(year, month + 1, 0);
  return date.getDate();
}

alert( getLastDayOfMonth(2012, 0) ); // 31
alert( getLastDayOfMonth(2012, 1) ); // 29
alert( getLastDayOfMonth(2013, 1) ); // 28
```

`new Date`�� �� ��° �Ű������� �⺻���� `1`�Դϴ�. �׷��� � ���ڸ� �Ѱ��൵ �ڹٽ�ũ��Ʈ�� �̸� �ڵ� �������ݴϴ�. `0`�� �ѱ�� 'ù ��° ���� 1�� ��'�� �ǹ��ϰ� �˴ϴ�. �̴� '���� ���� ������ ��'�� �����մϴ�.