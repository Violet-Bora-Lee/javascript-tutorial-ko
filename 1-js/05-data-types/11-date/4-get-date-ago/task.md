importance: 4

---

# n�� �� '��' ����ϱ�

`date`�� �������� `days`�� �� '��'�� ��ȯ�ϴ� �Լ� `getDateAgo(date, days)`�� ��������, 

������ 20���̶�� `getDateAgo(new Date(), 1)`�� 19��, `getDateAgo(new Date(), 2)`�� 18�� ��ȯ�ؾ� �մϴ�.

`days`�� `365`�� ���� ����� �����ؾ� �մϴ�.

```js
let date = new Date(2015, 0, 2); // 2015�� 1�� 2��

alert( getDateAgo(date, 1) ); // 1, (2015�� 1�� 1��)
alert( getDateAgo(date, 2) ); // 31, (2014�� 12�� 31��)
alert( getDateAgo(date, 365) ); // 2, (2014�� 1�� 2��)
```

����: �Լ��� `date`�� �������� �ʾƾ� �մϴ�. 