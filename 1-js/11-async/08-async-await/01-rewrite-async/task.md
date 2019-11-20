
# async�� await�� ����ؼ� '�ٽ� ������' ���� ���ۼ��ϱ�

<info:promise-chaining> é�Ϳ��� �ٷ�� '�ٽ� ������(rethrow)' ���� ���ø� ����Ͻ� �̴ϴ�. �� ���ø� `.then/catch` ��� `async/await`�� ����� �ٽ� �ۼ��� ���ô�.

�׸��� `demoGithubUser` ���� �ݺ�(recursion)�� �ݺ���(loop)�� ����� �ۼ��ϵ��� �սô�. `async/await`�� ����ϸ� ���� �ۼ��� �� �ֽ��ϴ�.

```js run
class HttpError extends Error {
  constructor(response) {
    super(`${response.status} for ${response.url}`);
    this.name = 'HttpError';
    this.response = response;
  }
}

function loadJson(url) {
  return fetch(url)
    .then(response => {
      if (response.status == 200) {
        return response.json();
      } else {
        throw new HttpError(response);
      }
    })
}

// ��ȿ�� ����ڸ� ã�� ������ �ݺ��ؼ� username�� ���
function demoGithubUser() {
  let name = prompt("GitHub username�� �Է��ϼ���.", "iliakan");

  return loadJson(`https://api.github.com/users/${name}`)
    .then(user => {
      alert(`�̸�: ${user.name}.`);
      return user;
    })
    .catch(err => {
      if (err instanceof HttpError && err.response.status == 404) {
        alert("��ġ�ϴ� ����ڰ� �����ϴ�. �ٽ� �Է��� �ּ���.");
        return demoGithubUser();
      } else {
        throw err;
      }
    });
}

demoGithubUser();
```