# 클래스 확인: "instanceof"

`instanceof` 연산자는 객체가 특정 클래스에 속해 있는지 없는지를 검사합니다. 또한, 상속 관계에 있는지 확인합니다.

확인하는 기능은 많은 경우에서 필수적인데, 여기에 매개변수의 타입때문에 처리하는 법이 달라지는 *다형적인* 함수를 빌드하기 위해 사용할 것입니다.

## instanceof 연산자 [#ref-instanceof]

문법은 아래와 같습니다.
```js
obj instanceof Class
```

`obj`가 `Class`에 속해 있거나 상속하고 있다면, true를 반환합니다.

예시:

```js run
class Rabbit {}
let rabbit = new Rabbit();

// rabbit이 Rabbit class에 속해있는지 물어봅니다.
*!*
alert( rabbit instanceof Rabbit ); // true
*/!*
```

instanceof는 생성자 함수에서도 동작합니다.

```js run
*!*
// 클래스 대신 함수를 사용하였습니다.
function Rabbit() {}
*/!*

alert( new Rabbit() instanceof Rabbit ); // true
```

또한, `Array` 같은 클래스의 내장함수로써의 기능도 있습니다.

```js run
let arr = [1, 2, 3];
alert( arr instanceof Array ); // true
alert( arr instanceof Object ); // true
```

`arr`은 또한, `Object` class에 속해있다는 것을 기억하세요. `Array`는 원래 `Object`를 상속하고 있습니다.

보통, `instanceof` 연산자는 프로토타입 체인의 확인을 검사해줍니다. 또한, 정적 메서드`Symbol.hasInstance`로 커스텀 로직을 설정할수 있습니다.

`obj instanceof Class`의 알고리즘은 대략적으로 다음과 같이 동작합니다.

1. 정적 메서드 `Symbol.hasInstance`가 있다고 가정한다면, 그저 호출하면 됩니다. `Class[Symbol.hasInstance](obj)`. 결과는 `true` 나 `false`값을 반환할 것입니다. 다음은 `instanceof`를 어떻게 커스텀 하는지에 대한 예시입니다.

    예시:

    ```js run

    // instanceOf는 canEat 프로퍼티가 aniaml인지 확인하는 것입니다.
    class Animal {
      static [Symbol.hasInstance](obj) {
        if (obj.canEat) return true;
      }
    }

    let obj = { canEat: true };

    alert(obj instanceof Animal); // true: Animal에서 저희가 커스터마이즈한 Symbol.hasInstance가 호출 되었기 때문에 true를 리턴했습니다.
    ```

2. 대부분 클래스는 `Symbol.hasInstance`를 가지고 있지 않습니다. 이러한 경우, 일반적인 로직이 사용될 것입니다. `obj instanceOf Class`는 `Class.prototype`이 `obj` 프로토 체인 내부의 프로토타입 중 하나와 같은지 확인합니다.

    다시 말해서 하나를 다른것들과 같은지 비교하는 것입니다.
    ```js
    obj.__proto__ === Class.prototype?
    obj.__proto__.__proto__ === Class.prototype?
    obj.__proto__.__proto__.__proto__ === Class.prototype?
    ...
    // 이 중 하나라도 true라면 true를 리턴합니다.
    // 다시 말하면, 위의 해당되는 것이 하나도 없다면 false 입니다. 그 말은 프로토 체인의 끝에 도달한다는 것을 의미합니다.
    ```

   위의 예제에서 `rabbit.__proto__ === Rabbit.prototype`에서 true이기 때문에 즉시 응답을 줍니다.

    상속의 경우는 두 번째 단계에서 일치됩니다.

    ```js run
    class Animal {}
    class Rabbit extends Animal {}

    let rabbit = new Rabbit();
    *!*
    alert(rabbit instanceof Animal); // true
    */!*

    // rabbit.__proto__ === Rabbit.prototype
    *!*
    // rabbit.__proto__.__proto__ === Animal.prototype (일치!)
    */!*
    ```

여기에 `rabbit instanceof Animal`의 무엇과 `Animal.prototype`을 비교하는 지에 대한 그림이 있습니다.

![](instanceof.svg)

그런데, `objA`가 `objB`의 프로토타입의 체인 어딘가에 있다면, `true`를 리턴하는 [objA.isPrototypeOf(objB)](mdn:js/object/isPrototypeOf)라는 메서드가 있습니다.

`Class` 생성자 그 자체는 확인할 수 없지만, 오직 프르토타입 체인과 `Class.prototype`은 매우 중요하므로 확인해야 합니다.

언제 `prototype`프로퍼티가 객체가 생성된 후에 변화되는지에 따라 흥미로운 결과를 이끌어 낼수 있습니다.

아래와 같이 말이죠.

```js run
function Rabbit() {}
let rabbit = new Rabbit();

// 프로토타입을 변형했습니다.
Rabbit.prototype = {};

// ...더이상 Rabbit이 아닙니다!
*!*
alert( rabbit instanceof Rabbit ); // false
*/!*
```

## 보너스: 타입을 위한 Object.prototype.toString

이미 여러분들은 평범한 객체들이 `[object Object]`의 문자열로 변환되는 것에 대해 알고 있습니다.

```js run
let obj = {};

alert(obj); // [object Object]
alert(obj.toString()); // 같습니다.
```

그것은 바로 `toString`으로 구현되어 있습니다. 그러나 `toString`은 실질적으로 그것이 가진 기능보다 더 강력하게 만들어 줄 수 있는 몇 가지 숨겨진 특징들이 있습니다. 확장된 기능으로써 `typeof`을 사용할수 있는데, 이것은 `instanceof` 대신 사용할 수 있습니다.

이상하게 들리나요? 그럼 미스터리로 두죠.

[이곳](https://tc39.github.io/ecma262/#sec-object.prototype.tostring)에 명시되어 있듯이, 객체와 실행중인 다른 값의 컨텍스트로부터 내장함수 `toString`을 사용하여 추출할 수 있습니다. 그리고 그 결과는 그 객체가 가진 값에 의존합니다.

- 숫자의 경우, 그것은 `[object Number]`가 될 것입니다.
- 불린의 경우, 그것은 `[object Boolean]`이 될 것입니다.
- null의 경우, 그것은 `[object Null]`가 될 것입니다.
- `undefined`의 경우, 그것은 `[object Undefined]`가 될 것입니다.
- 배열의 경우, 그것은 `[object Array]`가 될 것입니다.
- 그외에 여러가지가 다양하게 있는데, 그것에 따라 맞춰집니다.

그럼 같이 한번 해결해볼까요?

```js run
// 편의를 위해 toString 메소드를 변수에 복사합니다.
let objectToString = Object.prototype.toString;

// 이것의 타입은 무엇일까요?
let arr = [];

alert( objectToString.call(arr) ); // [object *!*Array*/!*]
```

이번 챕터에서 [](info:call-apply-decorators)에 서술되어 있는 [call](mdn:js/function/call)는 함수 `objectToString`은 `this=arr` 컨텍스트에 실행하기 위해서 사용했습니다.

내부적으로, `toString` 알고리즘은 `this`를 검사하고, 일치하는 결과를 반환합니다. 더 많은 예시를 살펴봅시다.

```js run
let s = Object.prototype.toString;

alert( s.call(123) ); // [object Number]
alert( s.call(null) ); // [object Null]
alert( s.call(alert) ); // [object Function]
```

### Symbol.toStringTag

객체 `toString`의 기능은 특별한 객체 프로퍼티 `Symbol.toStringTag`를 사용함으로써 커스텀 될 수 있습니다.

예시:

```js run
let user = {
  [Symbol.toStringTag]: "User"
};

alert( {}.toString.call(user) ); // [object User]
```

대부분의 특정 환경에서만 동작하는 객체를 위해, 프로퍼티같은 것들이 있습니다. 여기에 몇 가지 예시가 있습니다.

```js run
// 특정 환경의 객체와 클래스를 위한 toStringTag :
alert( window[Symbol.toStringTag]); // window
alert( XMLHttpRequest.prototype[Symbol.toStringTag] ); // XMLHttpRequest

alert( {}.toString.call(window) ); // [object Window]
alert( {}.toString.call(new XMLHttpRequest()) ); // [object XMLHttpRequest]
```

여러분도 아시다시피, 그 결과는 정확히 `Symbol.toStringTag`(이 속성을 가지고 있다면)와 내부`[object ...]`로 감싸줍니다.

끝으로 여려분은 원시적인 데이터 타입에 대한 작업뿐만 아니라 내장된 객체를 위한 "스테로이드들에 대한 타입"를 가지고 있고, 심지어 그것들의 커스텀도 가능합니다.

`{}.toString.call` 대신 `instanceof` 내장된 객체를 단순히 타입 확인을 넘어 문자열로 타입을 얻기를 원할 때, 사용할 수 있습니다.

## 요약

여러분이 알고 있는 타입 확인 메서드를 요약해봅시다.

|               | 동작대상      |  리턴타입      |
|---------------|-------------|---------------|
| `typeof`      | primitives  |  string       |
| `{}.toString` | primitives, 내장된 객체, `Symbol.toStringTag`를 가진 객체   |       string |
| `instanceof`  | objects     |  true/false   |

여러분도 아시다시피,`{}.toString`은 기술적으로 `typeof`보다 '더 발전된' 것입니다.

그리고 `instanceof` 연산자는 클래스 계층에 작업하거나, 상속을 고려하여 클래스를 확인 할 때, 정말 큰 빛을 발합니다.
