# Chapter09. 타입 변환과 단축 평가

<br>

## 1. 타입 변환이란?

- 타입 변환의 종류

| 종류                                                         | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 명시적 타입 변환(explicit coercion), 타입 캐스팅(type casting) | 개발자가 **의도적으로** 값의 타입을 변환하는 것              |
| 암묵적 타입 변환(implicit coercion), 타입 강제 변환(type coercion) | 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 **자동** 변환하는 것 |

```javascript
// 명시적 타입 변환

var x = 10;
var str = x.toString();
console.log(typeof str, str); // 'string' '10'
console.log(typeof x, x); // 'number' 10

// 암묵적 타입 변환

var y = 20;
var str = y + '';
console.log(typeof str, str); // 'string' '20'
console.log(typeof y, y); // 'number' 20
```

- 위 예시에서 명시적 타입 변환, 암묵적 타입 변환 모두 기존 원시 값을 직접 변경하는 것은 아니다.
  - 왜냐하면 원시 값은 변경 불가능한 값이기 때문이다.
  - 즉, 타입 변환은 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것이고 그 새로운 값을 원래 변수에 재할당하지 않는다.

---

### :heavy_plus_sign: 참조 타입 중 하나인 배열을 정렬할 때는 원본 데이터가 변할 수도 있다.

> 타입 변환과는 약간 동떨어진 내용일 수 있으나 타입 변환 중 변수에 재할당하지 않는 부분과 관련지어 함께 생각해보면 좋은 내용일 것 같아서 작성해보았다.

- 원시 값을 타입 변환하게 되면 새롭게 생성된 원시 값을 원래 변수에 재할당하지 않는다.
- 하지만 아래 예시처럼 배열을 정렬하는 `sort` 메서드를 수행하면 원본 배열이 정렬된 상태로 변하게 된다.

```javascript
const arr = [3, 4, 2, 5];
const sortedArr = arr.sort();
console.log(arr); // [2, 3, 4, 5]
console.log(sortedArr); // [2, 3, 4, 5]
```

- 만약 원본 배열의 순서는 그대로 유지하고 싶다면 아래 예시처럼 spread operator를 이용해서 새로운 배열로 복사한 후 정렬하면된다.

```javascript
const arr = [3, 4, 2, 5];
const sortedArr = [...arr].sort();
console.log(arr); // [3, 4, 2, 5]
console.log(sortedArr); // [2, 3, 4, 5]
```

---

- 자신이 작성한 코드에서 암묵적 타입 변환이 발생하는지, 발생한다면 어떤 타입의 어떤 값으로 변환되는지, 그리고 타입 변환된 값으로 표현식이 어떻게 평가될 것인지 예측 가능해야 한다.
  - 만약 타입 변환 결과를 예측하지 못하거나 예측이 결과와 일치하지 않는다면 오류를 생산할 가능성이 높아진다.
- 명시적 타입 변환, 암묵적 타입 변환 중 어떤 것이 제일 좋다고 명확히 규정짓기는 어렵다.
  - 중요한 것은 코드를 예측할 수 있어야 한다는 것이다.
  - 동료가 작성한 코드를 정확히 이해할 수 있어야 하고 자신이 작성한 코드도 동료가 쉽게 이해할 수 있어야 한다.

<br>

## 2. 암묵적 타입 변환(implicit coercion)

- 자바스크립트 엔진이 표현식을 평가할 때 개발자의 의도와는 상관없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환하는 것을 암묵적 타입 변환이라고 한다.
- 암묵적 타입 변환이 발생하면 문자열, 숫자, 불리언과 같은 원시 타입 중 하나로 타입을 자동 변환한다.

<br>

### (1) 문자열 타입으로 변환

- `+` 연산자는 피연산자 중 <b><u>하나 이상이 문자열인 경우</u></b> 문자열 연결 연산자로 동작한다.
  - 자바스크립트 엔진은 문자열 연결 연산자 표현식을 평가하기 위해 문자열 연결 연산자의 피연산자 중에서 문자열 타입이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환한다.

```javascript
1 + '2' // '12'
```

- 연산자의 표현식의 피연산자만이 암묵적 타입 변환의 대상이 되는 것은 아니다.
  - 예를 들어, ES6에서 도입된 템플리 리터럴의 표현식 삽입은 표현식의 평가 결과를 문자열 타입으로 암묵적 타입 변환한다.

```javascript
`1 + 1 = ${1 + 1}` // "1 + 1 = 2"
```

- 문자열 타입이 아닌 값을 문자열 타입으로 암묵적 타입 변환을 수행할 때 동작하는 예시

  - 숫자 타입

  ```javascript
  0 + '' // '0'
  -0 + '' // '0'
  1 + '' // '1'
  -1 + '' // '-1'
  NaN + '' // 'NaN'
  Infinity + '' // 'Infinity'
  -Infinity + '' // '-Infinity'
  ```

  ```javascript
  // 연산자 우선순위, 결합 순서에 의해 어떤 순서로 문자열 연결 연산자를 더하느냐에 따라 결과값이 달라진다.
  1 + 4 + '' // '5'
  1 + '' + 4 // '14'
  1 + '4' + '' // 14'
  '1' + 4 + '' // '14'
  ```

  - 불리언 타입

  ```javascript
  true + '' // 'true'
  false + '' // 'false'
  
  true + 1 // 2
  true + '1' // 'true1'
  ```

  - `null`, `undefined` 타입

  ```javascript
  null + '' // 'null'
  undefined + '' // 'undefined'
  ```

  - 심벌 타입

  ```javascript
  (Symbol()) + '' // TypeError: Cannot convert a Symbol value to a string
  ```

  - 참조 타입

  ```javascript
  // 자바스크립트에서 객체의 대부분의 암묵적 타입 변환은 '[object Object]'로 변환된다.
  // 모든 자바스크립트 객체는 'toString' 메서드를 상속받는다.
  // 상속받은 'toString' 메서드는 객체가 문자열 타입으로 변해야 할 때 쓰인다.
  ({}) + '' // '[object Object]'
  
  Math + '' // '[object Math]'
  
  // 배열에서 상속된 'toString' 메서드는 객체와 약간 다르게 동작한다.
  // 배열의 'join' 메서드를 호출한 것과 비슷한 방식으로 동작한다.
  [] + '' // ''
  [1, 2, 3, 4] + '' // '1,2,3,4'
  8 + [2] // '82'
  
  (function(){}) + '' // 'function(){}'
  
  Array + '' // 'function Array() { [native code] }'
  ```

<br>

### (2) 숫자 타입으로 변환

- 산술 연산자는 숫자 값을 만드는 역할을 한다.
  - 산술 연산자의 모든 피연산자는 코드 문맥상 모두 숫자 타입이어야 한다.

```javascript
1 - '1' // 0
1 * '10' // 10
1 / 'one' // NaN
```

- 자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 <b>산술 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로</b> 암묵적 타입 변환한다.
  - 이때 피연산자를 숫자 타입을 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 표현식의 평가 결과는 `NaN`이 된다.
- 또한 피연산자를 숫자 타입으로 변환해야 할 문맥은 산술 연산자뿐만 아니라 비교 연산자도 해당된다.
  - 비교 연산자는 피연산자의 크기를 비교하므로 모든 피연산자는 코드의 문맥상 모두 숫자 타입이어야 한다.

```javascript
'1' > 0 // true
'1' < '2' // true
```

- 그리고 `+` 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행하는 가장 간단한 방법이고 자주 사용된다.

- 숫자 타입으로 암묵적 타입 변환을 수행할 때 동작하는 예시

  - 문자열 타입

  ```javascript
  +'' // 0
  +'0' // 9
  +'1' // 1
  +'string' // NaN
  ```

  - 불리언 타입

  ```javascript
  +true // 1
  +false // 0
  ```

  - `null`, `undefined` 타입

  ```javascript
  +null // 0
  +undefined // NaN
  ```

  - 심벌 타입

  ```javascript
  +Symbol() // TypeError: Cannot convert a Symbol value to a number
  ```

  - 참조 타입

  ```javascript
  +{} // NaN
  +[] // 0
  +[2] // 2
  +['2'] // 2
  +[1, 2] // NaN
  +(function(){}) // NaN
  ```

- <b>빈 문자열, 빈 배열, `null`, `false`는 0으로, `true`는 1로 객체와 빈 배열이 아닌 배열, `undefined`는 변환되지 않아 `NaN`이 된다</b>는 것에 주의하자.
  
  - 숫자 타입으로 변환되는 내부 상세 로직은 `3. 명시적 타입 변환(explicit coercion)` 절이 끝난 후에 내부 로직을 살펴보면서 자세히 더 살펴보기로 하자.

<br>

### (3) 불리언 타입으로 변환

- `if` 문이나 `for` 문과 같은 제어문 또는 삼항 조건 연산자의 조건식은 불리언 값, 즉 논리적 참/거짓으로 평가되어야 하는 표현식이다.
  - 자바스크립트 엔진은 <b>조건식의 평가 결과를 불리언 타입으로</b> 암묵적 타입 변환한다.
- 이 때 자바스크립트 엔진은 불리언 타입이 아닌 <b>Truthy 값(참으로 평가되는 값) 또는 Falsy 값(거짓으로 평가되는 값)으로 구분</b>한다.
  - 즉, 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 Truthy 값은 `true`로, Falsy 값은 `false`로 암묵적 타입 변환된다.
  - `true`로 평가되는 Truthy 값은 무수히 많으므로 `false`로 평가되는 Falsy 값을 알아두면 나머지는 Truthy 값으로 생각하면 된다
  - Falsy 값 : `false`, `undefined`, `null`, `0`, `-0`, `NaN`, `''`(빈 문자열)
  - <b>주의할 점은 음수는 Truthy 값에 해당한다.</b>
- 또한 논리 부정 연산자(`!`)를 이용하여 불리언 타입으로 암묵적 타입 변환을 할 수 있다.

```javascript
// 전달받은 인수가 Falsy 값이면 true, Truthy 값이면 false를 반환한다.
function isFalsy(v) {
  return !v;
}

[false, undefined, null, 0, NaN, ''].forEach((v) => console.log(isFalsy(v))); // 모두 true 반환
```

- 주의해야할 점은 <b>문자열 타입의 숫자 `0`과 빈 배열(`[]`) 그리고 빈 객체(`{}`)는 Truthy 값</b>이라는 것이다.
  - <b>보통 불리언 타입으로 변환하기 위해 아래 `isTruthy` 함수와 같이 논리 부정 연산자를 두 번 사용한다.</b>

```javascript
// 전달받은 인수가 Truthy 값이면 true, Falsy 값이면 false를 반환한다.
function isTruthy(v) {
  return !!v;
}

['0', [], {}].forEach((v) => console.log(isTruthy(v))); // 모두 true 반환
```

<br>

## 3. 명시적 타입 변환(explicit coercion)

- 개발자의 의도에 따라 명시적으로 타입을 변환하는 것을 명시적 타입 변환이라고 한다.
  - 그 방법으로 표준 빌트인 생성자 함수를 `new` 연산자 없이 호출하는 방법, 빌트인 메서드를 사용하는 방법, 암묵적 타입 변환을 이용하는 방법이 있다.
  - 이 때 타입 변환할 때 내부 동작은 `2. 암묵적 타입 변환(implicit coercion)` 절에서 살펴본 내용과 거의 동일하다.
  - 이번 절에서는 이전 절에서 등장하지 않은 방법에 대해서만 소개하고자 한다.

<br>

### (1) 문자열 타입으로 변환

#### :round_pushpin: `String` 생성자 함수를 `new` 연산자 없이 호출하는 방법

```javascript
// 숫자 타입 => 문자열 타입
String(1); // '1'
String(NaN); // 'NaN'
String(Infinity); // 'Infinity'

// 불리언 타입 => 문자열 타입
String(true); // 'true'
String(false); // 'false'

// 참조 타입 => 문자열 타입
String([]); // ''
String([1, 2, 3, 4]); // '1,2,3,4'
String({}); // '[object Object]'
```

<br>

#### :round_pushpin: `Object.prototype.toString` 메서드를 사용하는 방법

```javascript
// 숫자 타입 => 문자열 타입
(1).toString(); // '1'
(NaN).toString(); // 'NaN'
(Infinity).toString(); // 'Infinity'

// 불리언 타입 => 문자열 타입
(true).toString(); // 'true'
(false).toString(); // 'false'

// 참조 타입 => 문자열 타입
([]).toString(); // ''
([1, 2, 3, 4]).toString(); // '1,2,3,4'
({}).toString(); // '[object Object]'
```

<br>

### (2) 숫자 타입으로 변환

#### :round_pushpin: `Number` 생성자 함수를 `new` 연산자 없이 호출하는 방법

```javascript
// 문자열 타입 => 숫자 타입
Number('0'); // 0
Number('-123'); // -123
Number('12.34'); // 12.34
Number('12.00'); // 12
Number('12.30'); // 12.3
Number('12.34km'); // NaN

// 불리언 타입 => 숫자 타입
Number(true); // 1
Number(false); // 0

// 참조 타입 => 숫자 타입
Number({}); // NaN
Number([]); // 0
Number([2]); // 2
Number(['2']); // 2
Number([1, 2]); // NaN
Number([Infinity]); // Infinity
```

<br>

#### :round_pushpin: `parseInt`, `parseFloat` 함수를 사용하는 방법

- `parseInt` 함수는 정수 형태로, `parseFloat` 함수는 부동소수점 실수 형태로 반환한다.

  - 단, 이 방법은 <b>문자열을 숫자 타입으로 변환할 때만</b> 가능하다.

- 만약, 함수의 인자로 문자열이 아닌 다른 타입을 작성하는 경우 아래와 같이 함수 인자로 `'string'` 타입만 올 수 있다는 에러 메시지를 볼 수 있다.

  - `parseInt(1);` 구문에서 출력되는 error 메시지 및 `parseInt` 함수 설명

    ![parseInt](https://user-images.githubusercontent.com/52685250/149652212-5ecddb95-eeeb-4b35-8c90-7567cbaaa021.png)

  - `parseFloat(1);` 구문에서 출력되는 error 메시지 및 `parseFloat` 함수 설명

    ![parseFloat](https://user-images.githubusercontent.com/52685250/149652213-64939a57-76d5-4695-a6d2-583de4a0f7c9.png)

- 또한 추가적으로 `parseInt` 함수의 경우 두 번째 인자로 몇 진법으로 표시할지 결정하는 `radix` 값을 넘길 수 있다.

  - `radix` 인자는 `?` 에 의해 optional로 넣어도 되고 안 넣어도 무방하지만 eslint의 기본 설정에서는 인자를 넘기라고 되어 있다.
  - 아무 값도 넘기지 않으면 기본값은 `10`이 되어 십진법으로 반환된다.
  - 만약 이 eslint 옵션을 해당 line에만 무효화시키고 싶은 경우 아래와 같이 eslint 전용 주석을 그 위에 추가하면 된다.

  ```javascript
  // eslint-disable-next-line radix
  parseInt(1);
  ```

  - `parseInt` 함수의 `radix` 인자에 대한 자세한 설명은 [MDN 공식 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/parseInt#%EC%84%A4%EB%AA%85)를 참고하자.

```javascript
parseInt('0'); // 0
parseInt('-123'); // -123
// parseInt 함수는 정수를 반환한다는 점 주의하자.
parseInt('12.34'); // 12
parseFloat('12.34'); // 12.34
parseInt('hello'); // NaN
parseFloat('hello'); // NaN
```

---

### :heavy_plus_sign: `Number` 생성자 함수 vs `parseInt` 함수

- 숫자 타입으로 변환하는 방법 두 가지를 살펴보았다.
  - 하지만 `Number` 생성자 함수와 `parseInt` 함수에는 약간의 차이가 있다.
- 우선 두 방법 모두 문자열을 인자로 받으면 해당 문자열을 숫자로 바꿔주는 부분은 동일하다.

```javascript
Number('1234'); // 1234
parseInt('1234'); // 1234
```

- 하지만 아래 예시처럼 문자열이 숫자 아닌 경우에는 출력되는 결과가 차이가 있다.
  - `Number` 생성자 함수는 파싱할 수 없는 문자가 하나라도 포함된 경우 무조건 `NaN`을 반환하고 `parseInt` 함수는 문자열이 숫자로 시작하는 경우 숫자가 끝날 때까지만 파싱하여 형 변환을 한다.
  - `parseInt` 함수도 시작이 숫자 형태가 아니라면 `Number` 함수와 마찬가지로 `NaN`을 반환한다.

```javascript
Number('1000km'); // NaN
parseInt('1000km'); // 1000

Number('거리:1000km'); // NaN
parseInt('거리:1000km'); // NaN

Number('1,000'); // NaN
parseInt('1,000'); // 1
```

---

#### :round_pushpin: `*` 산술 연산자를 이용하는 방법

```javascript
// 문자열 타입 => 숫자 타입
'0' * 1; // 0
'-123' * 1; // -123
'12.34' * 1; // 12.34
'12.00' * 1; // 12

// 불리언 타입 => 숫자 타입
true * 1; // 1
false * 0; // 0

// null, undefined 타입
null * 1; // 0
undefined * 1; // NaN

// 참조 타입
[] * 1; // 0
[2] * 1; // 2
['34'] * 1'; // 34
['3'] * ['2']; // 6
```

<br>

### (3) 불리언 타입으로 변환

#### :round_pushpin: `Boolean` 생성자 함수를 `new` 연산자 없이 호출하는 방법

```javascript
// 문자열 타입 => 불리언 타입
Boolean('*'); // true
Boolean(''); // false
Boolean('false'); // true

// 숫자 타입 => 불리언 타입
Boolean(0); // false
Boolean(1); // true
Boolean(-1); // true
Boolean(NaN); // false
Boolean(Infinity); // true

// null, undefined 타입 => 불리언 타입
Boolean(null); // false
Boolean(undefined); // false

// 참조 타입 => 불리언 타입
Boolean({}); // true
Boolean([]); // true
```

<br>

## 4. 타입 변환 내부 로직 살펴보기

> 이번 절은 책에 나와 있지 않은 내용이지만 타입 변환과 관련하여 더 공부하면서 새롭게 알게 된 내용들을 추가했다.

(작성중...)
