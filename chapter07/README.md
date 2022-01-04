# Chapter07. 연산자

<br>

## :hand: Intro - 연산자, 피연산자

- 연산자(operator)는 하나 이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 타입, 지수 연산 등을 수행해 하나의 값을 만든다.
- 이때 연산의 대상을 피연산자(operand)라 한다.
- 피연산자는 값으로 평가될 수 있는 표현식이어야 한다.
- 그리고 피연산자와 연산자의 조합으로 이뤄진 연산자 표현식도 값으로 평가될 수 있는 표현식이다.
- 피연산자 = '값' 이라는 명사의 역할 / 연산자 = '피연산자를 연산하여 새로운 값을 만든다'라는 동사의 역할

<br>

## 1. 산술 연산자(arithmetic operator)

> 피연산자를 대상으로 수학적 계산을 수행해 새로운 숫자 값을 만들고 만약 산술 연산이 불가능한 경우, `NaN`을 return 한다.

### (1) 이항 산술 연산자

- 예전부터 익히 사용해왔던 기본적인 사칙연산이 이에 해당한다.
- `+`(덧셈), `-`(뺄셈), `*`(곱셈), `/`(나눗셈), `%`(나머지) 연산자가 있다.

<br>

### (2) 단항 산술 연산자

- 증감(`++`/`--`) 연산자는 피연산자의 값을 변경하는 부수 효과가 있고 피연산자의 위치에 따라 의미하는 바가 다르다. 아래 예시를 통해 자세히 살펴보자.

```javascript
var x = 1;
var result;

// 선할당 후증가
result = x++;
console.log(result, x); // 1 2

// 선증가 후할당
result = ++x;
console.log(result, x); // 3 3

// 선할당 후감소
result = x--;
console.log(result, x); // 3 2

// 선감소 후할당
result = --x;
console.log(result, x); // 1 1
```

---

### :heavy_plus_sign: 증감 연산자(`++`/`--`) 대신에 할당 연산자(`+=`/`-=`)를...

- 자바스크립트 언어로 애플리케이션을 개발하면서 증감 연산자(`++`/`--`)를 사용하는 코드가 있을 때 eslint 옵션을 킨 경우 증감 연산자 부분에 아래와 같은 error를 볼 수 있다.

![eslint](https://user-images.githubusercontent.com/52685250/147877025-5498e224-3339-4857-9a34-a25e86435fde.png)

- eslint는 증감 연산자를 반기지 않는다.
  - 왜냐하면 증감 연산자는 자동으로 세미콜론이 추가되는 대상이여서 예상치 못하게 코드의 흐름을 변경시켜 의도하지 않은 값의 증가나 감소를 일으키는 에러를 발생시킬 가능성이 있다고 한다.
  - 상세 내용은 [여기](https://eslint.org/docs/rules/no-plusplus)서 확인할 수 있다.
- 또한 일부 개발자들 사이에서는 할당과 연산 중 어느 것이 먼저인지 파악하는 것이 번거로워 증감 연산자를 꺼려한다는 의견도 있다. ([관련 stackoverflow](https://stackoverflow.com/questions/971312/why-avoid-increment-and-decrement-operators-in-javascript))
- 그래서 eslint에서는 <b>할당 연산자 사용할 것을 권장</b>하고 있다.

``` javascript
// Bad
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// Good
for (let i = 0; i < 5; i += 1) {
  console.log(i);
}
```

---

- `+` 단항 연산자는 숫자 타입인 피연산자에 어떠한 효과도 없다.
  - 하지만 숫자 타입이 아닌 피연산자에 사용하면 피연산자를 숫자 타입으로 변환하여 반환한다.
  - 이때 피연산자를 직접 변경하는 것이 아니라 숫자 타입으로 변환된 새로운 값을 생성해서 반환한다.
  - 그래서 이 연산자는 부수 효과는 없다.

```javascript
+10; // 10
+(-10); // 10

// v1의 경우 문자열을 숫자로 타입 변환이 가능하다.
var v1 = '123';
console.log(+v1); // 123

// boolean 값은 숫자 타입으로 변환한다.
var v2 = true;
console.log(+v2); // 1

var v3 = false;
console.log(+v3); // 0

// v4의 경우 문자열을 숫자로 타입 변환할 수 없으므로 NaN을 반환한다.
var v4 = 'wow';
console.log(+v4); // NaN
```

```javascript
// 아래 코드는 과연 어떤 결과를 반환할까? 한번 생각해보자.
var v5 = ['34'];
console.log(+v5); // ?

var v6 = ['34', '5'];
console.log(+v6); // ?

var v7 = undefined;
console.log(+v7); // ?

var v8 = null;
console.log(+v8); // ?
```

- `-` 단항 연산자는 피연산자의 부호를 반전한 값으로 반환한다.
  - `+` 단항 연산자와 마찬가지로 숫자 타입이 아닌 피연산자에 사용하면 피연산자의 숫자 타입을 변환하여 반환한다.
  - 이때 피연산자를 변경하는 것은 아니고 부호를 반전한 값을 생성해 반환한다. 따라서 부수 효과는 없다.

```javascript
-(-10); // 10
-'10'; // -10
-true // -1
-'wow'; // NaN
```

<br>

### (3) 문자열 연결 연산자

- `+` 연산자는 피연산자 중 하나 이상이 문자열인 경우 문자열 연결 연산자로 동작한다.
  - 그 외의 경우는 산술 연산자로 동작한다.

```javascript
1 + '2'; // '12'
1 + 2; // 3
1 + true; // 2
1 + null; // 1
```

- 암묵적 타입 변환(implicit coercion), 타입 강제 변환(type coercion)
  - 개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환된다.
  - 위 예제에서 `1 + true` 를 연산하면 자바스크립트 엔진은 암묵적으로 boolean 타입의 값인 `true`를 숫자 타입인 `1`로 타입을 강제로 변환한 후 연산을 수행한다.
- cf) 명시적 타입 변환
  - 개발자의 의도에 따라 명시적으로 타입을 변경하는 방법 중 하나이다.
  - 표준 빌트인 생성자 함수(`String`, `Number`, `Boolean`)를 `new` 연산자 없이 호출하는 방법이다.

```javascript
Number('123'); // 123
String(NaN); // 'NaN'
```

<br>

## 2. 할당 연산자(assignment operator)

- 우항에 있는 피연산자의 평가 결과를 좌항에 있는 변수에 할당한다.

```javascript
var x = 5;

x += 2;
console.log(x); // 7

x -= 2;
console.log(x); // 5

x *= 2;
console.log(x); // 10

x /= 2;
console.log(x); // 5

x %= 2;
console.log(x); // 1
```

```javascript
// 문자열 연결 연산자

var str = 'Hello!';
str += ' wow!';

console.log(str); // 'Hello! wow!'
```

- 할당문은 값으로 평가되는 표현식인 문으로서 할당된 값으로 평가된다.

```javascript
var x;
console.log(x = 10); // 10
```

- 따라서 할당문을 다른 변수에 할당할 수 있어서 이러한 특징을 활용해 여러 변수에 동일한 값을 연쇄 할당할 수 있다.

```javascript
var a, b, c;

// 연쇄 할당: 오른쪽에서 왼쪽으로 진행
a = b = c = 0;

console.log(a, b, c); // 0 0 0
```

<br>

## 3. 비교 연산자(comparison operator)

### (1) 동등 비교 연산자(loose equality operator) - `==`

- 동등 비교 연산자는 좌항과 우항의 피연산자를 비교할 때 <b>먼저 암묵적 타입 변환을 통해 타입을 일치시킨 후 같은 값인지 비교</b>한다.
  - 따라서 좌항과 우항의 피연산자의 타입은 다르더라도 암묵적 타입 변환 후에 같은 값일 수 있다면 `true`를 반환한다.
- 이로 인해 결과를 예측하기 어렵고 실수하기 쉽다.
  - 그래서 동등 비교 연산자는 사용하지 않는 편이 좋다.

```javascript
5 == 5; // true
5 == '5'; // true
'0' == ''; // false
0 == ''; // true
0 == '0'; // true
false == '0'; // true
false == null; // false
false == undefined; // false
```

<br>

### (2) 일치 비교 연산자(strict equality operator) - `===`

- 일치 비교 연산자는 좌항과 우항의 피연산자가 타입도 같고 값도 같은 경우에 한하여 `true`를 반환한다.
  - <b>암묵적 타입 변환을 하지 않고 값을 비교</b>하기 때문에 예측하기 쉽다.

```javascript
5 === 5; // true
5 === '5'; // false
0 === ''; // false
```

- 단, 일치 비교 연산자에서 주의할 것은 `NaN`이다.
  - `NaN`은 자신과 일치하지 않는 유일한 값이다.
  - 따라서 숫자가 `NaN`인지 조사하려면 빌트인 함수 `isNaN`을 사용한다.

```javascript
NaN === NaN; // false
```

- 숫자 `0`도 주의해야 한다. 양의 `0`과 음의 `0`이 있는데 이들을 비교하면 `true`를 반환한다.

```javascript
0 === -0; // true
0 == -0; // true
```

---

### :heavy_plus_sign: `Object.is` 메서드

- 양의 `0`과 음의 `0`은 동등 비교 연산자와 일치 비교 연산자 모두 동일하다고 평가한다.
- 또한 동일한 값인 `NaN`과 `NaN`을 비교하면 다른 값이라고 평가한다.
- 그래서 이러한 이슈들을 해결하기 위해 ES2015에서 `Object.is` 메서드가 도입되었다.

```javascript
-0 === +0; // true
Object.is(-0, +0); // false

NaN === NaN; // false
Object.is(NaN, NaN); // true

// 특별한 경우 (출처 - Object.is 메서드 MDN 공식 문서)
Object.is(0, -0); // false
Object.is(-0, -0); // true
Object.is(NaN, 0/0); // true
```

---

<br>

### (3) 대소 관계 비교 연산자

```javascript
5 > 0; // true
5 > 5; // false
5 >= 5; // true
5 <= 5; // true
```

<br>

## 4. 삼항 조건 연산자(ternary operator)

```text
조건식 ? 조건식이 true일 때 반환할 값 : 조건식이 false일 때 반환할 값
```

```javascript
var x = 2;

var result = x % 2 ? '홀수' : '짝수';

console.log(result); // 짝수
```

- 삼항 조건 연산자 표현식은 <b>값으로 평가</b>할 수 있는 표현식인 문이고 `if ... else` 문은 표현식이 아닌 문으로 <b>값처럼 사용할 수 없다.</b>
- 조건에 따라 어떤 값을 결정해야 한다면 `if ... else` 문보다 삼항 조건 연산자 표현식을 사용하는 편이 유리하다.
- 하지만 조건에 따라 수행해야 할 문이 하나가 아니라 여러 개라면 `if ... else` 문의 가독성이 더 좋다.
- <b>무조건 짧다고 좋은 코드가 아니라는 것을 잊지 말자!</b>

<br>

## 5. 논리 연산자(logical operator)

- 논리합(`||`) 연산자 - `OR`

```javascript
true || true; // true
true || false; // true
false || true; // true
false || false; // false
```

- 논리곱(`&&`) 연산자 - `AND`

```javascript
true && true; // true
true && false; // false
false && true; // false
false && false; // false
```

- 논리 부정(`!`) 연산자 - `NOT`

```javascript
!true; // false
!false; // true
```

- 논리 부정 연산자의 암묵적 타입 변환

```javascript
!0; // true
!'hello'; // false
```

- 논리합 또는 논리곱 연산자 표현식의 평가 결과는 boolean 값이 아닐 수도 있다.
  - 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다.

```javascript
// 단축 평가
'abc' && 'def'; // 'def'
'abc' || 'def'; // 'abc'
```

<br>

## 6. 쉼표 연산자

- 왼쪽 피연산자부터 차례대로 피연산자를 평가하고 마지막 피연산자의 평가가 끝나면 마지막 피연산자의 평가 결과를 반환한다.

```javascript
var x, y, z;

x = 1, y = 2, z = 3; // 3
```

<br>

## 7. 그룹 연산자

- 소괄호(`()`)로 피연산자를 감싸는 그룹 연산자는 자신의 피연산자인 표현식을 가장 먼저 평가한다.
  - 따라서 그룹 연산자를 사용해서 연산자의 우선순위를 조절할 수 있다.
  - 그룹 연산자는 연산자 우선순위가 가장 높다.

``` javascript
10 * 2 + 3; // 23

10 * (2 + 3); // 50
```

<br>

## 8. `typeof ` 연산자

- 피연산자의 데이터 타입을 문자열로 반환한다.
- `"string"`, `"number"`, `"boolean"`, `"undefined"`, `"symbol"`, `"object"`, `"function"` 중 하나를 반환하며 참고로 `typeof` 연산자가 반환하는 문자열은 7개의 데이터 타입과 정확히 일치하지는 않으니 주의하자.

| x               | typeof x                             |
| --------------- | ------------------------------------ |
| `''`            | `"string"`                           |
| `1`             | `"number"`                           |
| `NaN`           | `"number"`                           |
| `true`          | `"boolean"`                          |
| `undefined`     | `"undefined"`                        |
| `Symbol()`      | `"symbol"`                           |
| `null`          | `"object"` <b>(주의!)</b>            |
| `[]`            | `"object"`                           |
| `{}`            | `"object"`                           |
| `new Date()`    | `"object"`                           |
| `/test/gi`      | `"object"`                           |
| `12n`           | `"bigint"` (ES202에서 새롭게 추가됨) |
| `function() {}` | `"function"`                         |

- 값이 `null` 타입인지 확인할 때는 `typeof` 연산자를 사용하지 말고 일치 연산자(`===`)를 사용하자.

```javascript
var foo = null;

typeof foo === null; // false
foo === null; // true
```

- 선언하지 않은 식별자를 `typeof` 연산자로 연산해 보면 `ReferenceError`가 발생하지 않고 `undefined`를 반환한다.

```javascript
// x를 선언한적이 없다고 가정
typeof x; // undefined
```

<br>

## 9. 지수 연산자(exponentiation operator)

- ES2016에서 도입된 지수 연산자는 좌항의 피연산자를 밑으로, 우항의 피연산자를 지수로 거듭 제곱하여 숫자 값을 반환한다.
- 지수 연산자가 도입되기 이전에는 `Math.pow` 메서드를 사용했다.
- `Math.pow` 메서드 대신에 지수 연산자를 사용해야 코드의 가독성이 매우 좋아진다.

```javascript
2 ** 2 ** 2; // 16
Math.pow(Math.pow(2, 2), 2); // 16
```

- 오죽하면 eslint에서도 `Math.pow` 메서드를 사용하려고 하면 아래와 같이 린트 에러 메시지를 보여준다.

```text
'Math.pow' is restricted from being used. Use the exponentiation operator (**) instead.
```

- 음수를 거듭제곱의 밑으로 사용해 계산하려면 다음과 같이 괄호를 묶어야 한다.

```javascript
(-5) ** 2; // 25
```

- 지수 연산자는 이항 연산자 중에서 우선순위가 가장 높다.

```javascript
2 * 5 ** 2; // 50
```

- 단, 지수 연산자는 연속으로 사용했을 때 뒤에서부터 적용된다.

```javascript
3 ** 1 ** 2; // 3

(3 ** 1) ** 2; // 9
3 ** (1 ** 2); // 3
```

<br>

## 10. 연산자 우선순위

- 연산자 우선순위란 여러 개의 연산자로 이뤄진 문이 실행될 때 연산자가 실행되는 순서를 말한다.
- 우선순위가 높을수록 먼저 실행된다.
- 참고로 연산자 우선순위는 내용이 많아 [MDN 공식문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)로 대체한다.
- 이 모든 우선순위를 기억하는 것은 너무 어렵고 실수하기도 쉽다.
- 따라서 기억에 의존하기보다는 연산자 우선순위가 가장 높은 <b>그룹 연산자를 사용하여 우선순위를 명시적으로 조절하는 것을 권장</b>한다.
