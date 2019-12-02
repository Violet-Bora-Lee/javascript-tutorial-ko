importance: 5

---

# 이상한 instanceof

<<<<<<< HEAD
아래 예시에서 `a`는 `B()`를 통해 생성하지 않았습니다. 그런데도 `instanceof`는 왜 `true`를 반환할까요? 
=======
In the code below, why does `instanceof` return `true`? We can easily see that `a` is not created by `B()`.
>>>>>>> 47d186598add3a0ea759615596a12e277ce8fb5a

```js run
function A() {}
function B() {}

A.prototype = B.prototype = {};

let a = new A();

*!*
alert( a instanceof B ); // true
*/!*
```
