
���Ӽ����� ���� �����Դϴ�. `demoGithubUser`���� `.catch`�� `try...catch`�� ��ü�ϰ� �ʿ��� ���� `async/await`�� �߰��ϸ� �˴ϴ�. 

```js run
class HttpError extends Error {
  constructor(response) {
    super(`${response.status} for ${response.url}`);
    this.name = 'HttpError';
    this.response = response;
  }
}

async function loadJson(url) {
  let response = await fetch(url);
  if (response.status == 200) {
    return response.json();
  } else {
    throw new HttpError(response);
  }
}

// ��ȿ�� ����ڸ� ã�� ������ �ݺ��ؼ� username�� ���
async function demoGithubUser() {

  let user;
  while(true) {
    let name = prompt("GitHub username�� �Է��ϼ���.", "iliakan");

    try {
      user = await loadJson(`https://api.github.com/users/${name}`);
      break; // ������ �����Ƿ� �ݺ����� �������ɴϴ�.
    } catch(err) {
      if (err instanceof HttpError && err.response.status == 404) {
        // �� â�� �� ���Ŀ� �ݺ����� ��� ���ϴ�.
        alert("��ġ�ϴ� ����ڰ� �����ϴ�. �ٽ� �Է��� �ּ���.");
      } else {
        // �� �� ���� ������ �ٽ� �������ϴ�.
        throw err;
      }
    }      
  }


  alert(`�̸�: ${user.name}.`);
  return user;
}

demoGithubUser();
```