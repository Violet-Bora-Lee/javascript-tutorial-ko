importance: 3

---

# �ּ� ���� �±�

��ũ��Ʈ�� ���� ����� �����غ�����.

```html
<script>
  let body = document.body;

  body.innerHTML = "<!--" + body.tagName + "-->";

  alert( body.firstChild.data ); // �� â�� � ������ ��µɱ��?
</script>
```