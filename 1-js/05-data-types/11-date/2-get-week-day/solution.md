`date.getDay()` �޼���� ������ ��Ÿ���� ���ڸ� ��ȯ�մϴ�(�Ͽ��Ϻ��� ����).

������ ��� �迭�� �����, `date.getDay()`�� ȣ���� ��ȯ���� ���� �ε����� ����ϸ� ���ϴ� ����� ������ �� �ֽ��ϴ�.

```js run demo
function getWeekDay(date) {
  let days = ['SU', 'MO', 'TU', 'WE', 'TH', 'FR', 'SA'];

  return days[date.getDay()];
}

let date = new Date(2014, 0, 3); // 2014�� 1�� 3��
alert( getWeekDay(date) ); // FR
```

--2019-11-15--