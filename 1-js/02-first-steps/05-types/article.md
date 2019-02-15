# 데이터 타입(Data types)

자바스크립트의 변수는 어떤 데이터든지 담을 수 있습니다. 변수는 어떤 순간에 문자열일 수 있고 다른 순간엔 숫자가 될 수도 있습니다:

```js
// no error
let message = "hello";
message = 123456;
```

이와 같이 데이터 타입(자료형)은 있지만 변수가 어느 데이터 타입에도 묶여있지 않는 것을 "동적 타이핑(dynamically typed)"이라 하고, 프로그래밍 언어중엔 이런 동적 타이핑을 허용하는 언어가 있습니다.

자바스크립트에는 일곱 가지 기본 데이터 타입이 있습니다. 이번 주제에선 기본적인 데이터 타입에 대해 전반적으로 다루고, 이후 주제에서 각 데이터 타입을 자세히 다루도록 하겠습니다.

## 숫자 타입

```js
let n = 123;
n = 12.345;
```

*숫자(number)* 타입은 정수 및 부동소수점 숫자를 나타냅니다.

곱셈 `*`, 나눗셈 `/`, 덧셈 `+`, 뺄셈 `-` 등과 같이 숫자와 관련된 다양한 연산이 존재합니다.

일반 숫자 외에도 숫자형에 속하는 "특수 숫자 값(special numeric values)"이 있습니다 : `Infinity`, `-Infinity` 및 `NaN`.

- `Infinity`는 수학에서의 [무한대](https://en.wikipedia.org/wiki/Infinity) ∞를 나타냅니다. 어떤 숫자보다 큰 특별한 값입니다.

    어느 숫자든 0으로 나누면 무한대를 얻을 수 있습니다:

    ```js run
    alert( 1 / 0 ); // 무한대
    ```

    직접 참조해서 얻을 수도 있습니다:

    ```js run
    alert( Infinity ); // 무한대
    ```
- `NaN`은 계산상의 오류를 나타냅니다. 다음과 같이 부정확하거나 정의되지 않은 수학 연산의 결과입니다.

    ```js run
    alert( "숫자가 아님" / 2 ); // NaN, 이런 나눗셈 연산은 에러를 발생시킵니다
    ```

    `NaN`은 여간해선 바뀌지 않습니다. `NaN` 값을 가지고 추가적인 연산을 하면 결국 `NaN`이 반환됩니다 :

    ```js run
    alert( "숫자가 아님" / 2 + 5 ); // NaN
    ```

    연산 과정 어디에선가 `NaN`이 나왔다면 이는 연산의 모든 결과물에 영향을 미칩니다.

```smart header="수학 연산은 안전합니다."
자바스크립트에서 수학 연산은 "안전"합니다. 0으로 나누고 숫자가 아닌 문자열을 숫자로 취급하는 등, 모든 연산이 가능합니다.

스크립트는 치명적인 에러("die")를 내뿜으며 멈추지 않습니다. 최악의 경우엔 `NaN`을 결과로 얻습니다.
```

특수 숫자 값은 공식적으로 "숫자"형에 속합니다. 이 값들이 현실 세계에선 숫자가 아니지만 말이죠.

 숫자를 다루는 방법에 대해선 <info:number> 주제에서 자세히 알아볼 것입니다.

## 문자열(A string) 타입

자바스크립트에선 문자열을 따옴표로 묶습니다.

```js
let str = "Hello";
let str2 = '자';
let phrase = `can embed ${str}`;
```

자바스크립트에는 세 가지 타입의 따옴표를 사용합니다.

1. 큰따옴표: `"Hello"`.
2. 작은따옴표: `'Hello'`.
3. 역따옴표(백틱, Backticks): <code>&#96;Hello&#96;</code>.

큰따옴표와 작은따옴표는 "간단한" 따옴표입니다. 자바스크립트에서는 두 따옴표 사이에 차이점이 없습니다.

역 따옴표는 "확장된 기능"을 가진 따옴표입니다. 변수와 표현식을 `${…}`로 감싼 후 문자열 안에 넣을 수 있게 해줍니다. 다음과 같이 말이죠:

```js run
let name = "John";

// 변수를 문자열 중간에 삽입
alert( `Hello, *!*${name}*/!*!` ); // Hello, John!

// 표현식을 문자열 중간에 삽입
alert( `the result is *!*${1 + 2}*/!*` ); // the result is 3
```

`${…}` 내부의 표현식이 평가되고 난 후 그 결과는 문자열의 일부가 됩니다. `${…}` 안에는 무엇이든 들어갈 수 있습니다: `name` 같은 변수나 수학적 표현인 `1 + 2` 이외에도 좀 더 복잡한 것들을 넣을 수 있습니다. 

이런 방법은 역따옴표를 써야만 유효합니다. 다른 따옴표에는 이러한 삽입 기능이 없습니다!
```js run
alert( "the result is ${1 + 2}" ); // the result is ${1 + 2} (역 따옴표는 아무것도 하지 않습니다)
```

<info:string> 주제에서 문자열에 대해 더 자세하게 다루도록 하겠습니다. 

```smart header="*문자* 자료형은 없습니다."
일부 언어에는 문자 한 개를 저장하는 "문자(character)" 자료형이 있습니다. 예를 들어, C 언어와 Java에서의 `char`입니다.

자바스크립트에는 문자 자료형이 없습니다. `문자열(string)` 자료형만 있습니다. 문자열은 문자 하나 또는 많은 문자로 이루어져 있습니다.
```

## 불리언(A boolean) 타입

불리언 타입(논리 자료형)은 `true`와 `false` 두 가지 값만 가집니다.

이 자료형은 yes/no 값을 저장하는 데 사용됩니다. `true`는 "yes, correct"를 의미하고 `false`는 "no, incorrect"를 의미합니다.

예:

```js
let nameFieldChecked = true; // yes, name field is checked
let ageFieldChecked = false; // no, age field is not checked
```
불리언 값은 비교 후 결과를 저장하기도 합니다:

```js run
let isGreater = 4 > 1;

alert( isGreater ); // true (비교 결과: "yes")
```

<info:logical-operators> 주제에서 불리언에 대해 자세히 다루도록 하겠습니다.

## "null" 값

`null` 값은 위에 설명된 자료형 중 어느 것에도 속하지 않습니다.

`null` 값은 오로지 `null` 값만 포함하는 별도의 자료형을 형성합니다:

```js
let age = null;
```

자바스크립트에서 `null`은 다른 언어에서처럼 "존재하지 않는 객체에 대한 참조" 또는 "null pointer"를 나타내지 않습니다.

`null`은 단지 "nothing"(아무것도 아닌), "empty"(빈) 또는 "unknown"(알 수 없는)을 나타내기 위해 사용된 특별한 값입니다.

위의 코드는 어떤 이유로 `나이(age)`를 알 수 없거나 비어있는 경우를 표현합니다.

## "undefined" 값

`undefined` 값도 별도의 자료형을 만듭니다. `null`처럼 자신만의 고유한 자료형인 `undefined` 타입에 속하게 됩니다.

`undefined`은 "값이 할당되지 않음"을 의미합니다.

변수는 선언되었지만, 값을 할당하지 않았다면, 해당 변수엔 `undefined`이 할당됩니다:

```js run
let x;

alert(x); // shows "undefined"
```

기술적으로, 어떠한 변수에도 `undefined`를 할당할 수 있습니다 :

```js run
let x = 123;

x = undefined;

alert(x); // "undefined"
```

...하지만 권장 사항은 아닙니다. 변수가 "비어있거나(empty)" "알 수 없음(unknown)"을 나타내려면 `null`을 사용하고, 초기화되지 않은 변수(변수는 선언되었지만, 값을 할당하지 않은 변수)를 확인할 때에 `undefined`를 사용합니다.

## 객체와 심볼(Objects and Symbols)

`객체(object)` 타입은 특별합니다.

객체 타입을 제외한 다른 자료형은 (문자열이든 숫자든) 한 가지만 표현할 수 있기 때문에 원시(primitive) 자료형이라 부릅니다. 그에 반해서 객체는 데이터 컬렉션과 복잡한 개체(entity)를 저장하는 데 사용됩니다. 객체에 대해선 원시 자료형을 배우고 난 후 <info:object>에서 다루도록 하겠습니다.

`심볼(symbol)` 타입은 객체의 고유한 식별자(unique identifiers)를 만들 때 사용됩니다. 여기서 심볼에 대해 설명하면 튜토리얼의 완성도를 높일 수 있겠지만, 심볼 타입은 객체를 공부한 이후에 공부하는 것이 낫기 때문에 넘어가도록 하겠습니다. 

## typeof 연산자(The typeof operator) [#type-typeof] 

`typeof` 연산자는 인수(argument)의 자료형을 반환합니다. 자료형이 다를 때 값을 다르게 처리하고 싶거나 빠르게 변수의 데이터 타입을 알아내야 할 때 유용합니다.

`typeof` 연산자는 두 가지 형태의 syntax(구문)을 지원합니다 :

1. 연산자 형태(As an operator): `typeof x`.
2. 함수 형태(As a function): `typeof(x)`.

괄호가 있든 없든 작동하고, 결과도 같습니다.

`typeof x` 호출은 자료형을 나타내는 문자열을 반환합니다:

```js
typeof undefined // "undefined"

typeof 0 // "number"

typeof true // "boolean"

typeof "foo" // "string"

typeof Symbol("id") // "symbol"

*!*
typeof Math // "object"  (1)
*/!*

*!*
typeof null // "object"  (2)
*/!*

*!*
typeof alert // "function"  (3)
*/!*
```

마지막 세 줄은 추가 설명이 필요합니다:

1. `Math`은 수학 연산을 제공하는 내장 객체입니다. <info:number> 주제에서 학습할 예정입니다. 위 코드에서 `Math`는 객체 타입의 예로 사용되었습니다.
2. `typeof null`의 결과는 `"object"`입니다. 이는 잘못된 결과입니다. 공식적으로 인정되는 `typeof` 오류이지만 호환성을 위해 유지되고 있습니다. 물론 `null`은 객체가 아닙니다. 별도의 고유한 자료형을 가지는 특수 값입니다. 다시 말하지만, 이것은 언어의 오류입니다.
3. `alert`는 언어의 함수이기 때문에 `typeof alert`의 결과는 `"function"`입니다. 다음주제에서 함수(function)을 공부하면서 자바스크립트엔 "함수(function)" 타입이 없다는 걸 알아보겠습니다. 함수는 객체 타입에 속합니다. 하지만 `typeof`는 함수가 피연산자로 들어오면 이를 다르게 취급합니다. 형식적으로는 잘못되었지만, 실제 사용시 에는 이 특징이 매우 유용합니다. 

## 요약

자바스크립트에는 일곱 가지 기본 자료형이 있습니다.

- 정수 또는 부동 소수점 등의 모든 종류의 숫자를 나타내는 `숫자(number)` 타입
- 문자열을 위한 `문자열(string)` 타입. 문자열은 하나 혹은 그 이상의 문자로 구성되어 있으며, 단일 문자를 나타내는 별도의 자료형은 없음.
- `true`/`false`(참/거짓)을 나타내는 `불리언(boolean)` 타입.
- 알 수 없는 값을 위한 `null` 타입  -- 단일 값, `null` 을 가진 독립 자료형.
- 할당되지 않은 값을 위한 `undefined` 타입 -- 단일 값, `undefined`인 가진 독립 자료형.
- 더 복잡한 데이터 구조를 위한 `객체(object)` 타입
- 고유한 식별자를 위한 `심볼(symbol)` 타입

`typeof` 연산자는 어떤 자료형이 변수에 저장되어 있는지를 알려줍니다.

- 2가지 형식: `typeof x` 또는 `typeof(x)`.
- `"string"`과 같이 자료형의 이름을 가진 문자열을 반환합니다.
- For `null` returns `"object"` -- this is an error in the language, it's not actually an object.
- `null`의 typeof 연산은 `"object"`를 반환합니다. -- 이것은 언어상의 오류 입니다. null은 객체가 아닙니다. 

다음 주제에서는 원시자료형에 대해 살펴보고, 이 자료형이 익숙해지면 객체를 학습하도록 하겠습니다.