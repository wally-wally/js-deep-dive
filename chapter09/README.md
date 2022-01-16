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
- 또한 논리 부정 연산자(`!`)를 이용하여 불리언 타입으로 암묵적 타입 변환을 할 수 있다.

```javascript
// 전달받은 인수가 Falsy 값이면 true, Truthy 값이면 false를 반환한다.
function isFalsy(v) {
  return !v;
}

[false, undefined, null, 0, NaN, ''].forEach((v) => console.log(isFalsy(v))); // 모두 true 반환
```

- 주의해야할 점은 <b>문자열 타입의 숫자 `0`과 음수 값과 빈 배열(`[]`) 그리고 빈 객체(`{}`)는 Truthy 값</b>이라는 것이다.
  - <b>보통 불리언 타입으로 변환하기 위해 아래 `isTruthy` 함수와 같이 논리 부정 연산자를 두 번 사용한다.</b>

```javascript
// 전달받은 인수가 Truthy 값이면 true, Falsy 값이면 false를 반환한다.
function isTruthy(v) {
  return !!v;
}

['0', -1, [], {}].forEach((v) => console.log(isTruthy(v))); // 모두 true 반환
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

## 4. 타입 변환 내부 연산 자세히 살펴보기

> 이번 절은 책에 나와 있지 않은 내용이지만 타입 변환과 관련하여 더 공부하면서 새롭게 알게 된 내용들을 추가했다.
>
> 그 중에서도 자바스크립트에서 숫자가 아닌 값에서 <b><u>숫자로 강제 변환</u></b>할 때 내부적으로 어떤 일이 일어나는지 살펴보도록 하자.

### (1) 숫자로 타입 변환시 내부적으로 사용되는 주요 연산

- 숫자로 강제 변환할 때 내부적으로 사용되는 연산 세 가지를 먼저 살펴보도록 하자.
- 참고로 자바스크립트의 내부 로직은 원래 C++ 언어로 구성되어 있다.
- 하지만 이 로직을 자바스크립트 언어로 보기 쉽게 구현해놓은 코드가 있어 자바스크립트 버전으로 올려놓았다.

#### :round_pushpin: `Typeof(value)`

- 함수의 인자 값이 <b>어떤 type인지</b> 반환하는 함수이다.

```javascript
function TypeOf(value) {
  const result = typeof value;
  switch (result) {
    case 'function':
      return 'object';
    case 'object':
      if (value === null) {
        return 'null';
      } else {
        return 'object';
      }
    default:
      return result;
  }
}
```

<br>

#### :round_pushpin: `ToNumber(argument)`

- 함수의 인자 값을 <b>숫자로 변환</b>하는 내부 로직이다.

```javascript
function ToNumber(argument) {
  if (argument === undefined) {
    return NaN;
  } else if (argument === null) {
    return +0;
  } else if (argument === true) {
    return 1;
  } else if (argument === false) {
    return +0;
  } else if (TypeOf(argument) === 'number') {
    return argument;
  } else if (TypeOf(argument) === 'string') {
    return parseTheString(argument); // not shown here(내부 로직이 상당히 복잡하다고 함...)
  } else if (TypeOf(argument) === 'symbol') {
    throw new TypeError();
  } else if (TypeOf(argument) === 'bigint') {
    throw new TypeError();
  } else {
    // argument is an object
    const primValue = ToPrimitive(argument, 'number');
    return ToNumber(primValue);
  }
}
```

<br>

#### :round_pushpin: `ToPrimitive(input, hint)`

- 어떤 형태든지 <b>원시값(`number`, `string`, `boolean`, `null`, `undefined` 중 하나)으로 변환</b>하는 로직이다.
- 이 때 두 번째 인자인 `hint`에 의해 어떤 타입으로 변환할지 구분 기준을 세울 수 있다.
  - `'string'`, `'number'`, `'default'` 세 가지 중 하나가 올 수 있는데 `'string'`, `'number'`는 말 그대로 문자열, 숫자 타입으로 변환하겠다는 의미이다.
  - `'default'`는 연산자가 기대하는 자료형이 확실하지 않을 때를 의미하며 아주 드물게 발생한다.

```javascript
function ToPrimitive(input: any, hint: 'string' | 'number' | 'default' = 'default') {
  if (TypeOf(input) === 'object') {
    const exoticToPrim = input[Symbol.toPrimitive];
    if (exoticToPrim !== undefined) {
      const result = exoticToPrim.call(input, hint);
      if (TypeOf(result) !== 'object') {
        return result;
      }
      throw new TypeError();
    }
    if (hint === 'default') {
      hint = 'number';
    }
    return OrdinaryToPrimitive(input, hint);
  }
  // input is already primitive
  return input;
}
```

- 내부 로직 설명

| 케이스                                                       | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `input`의 타입이 `object`이고 `Symbol.toPrimitive` 메소드가 있는 경우 | 결과가 `object`이면 `TypeError`를 발생시키고 아니면 그 결과를 반환한다. |
| `input`의 타입이 `object`이고 `Symbol.toPrimitive` 메소드가 없는 경우 | 다음에서 설명할 `OrdinaryToPrimitive` 함수를 실행하고 이 때 `hint` 값은 별도의 세팅이 없는 경우 `'number'`로 설정된다. |
| `input` 타입이 `object`가 아닌 경우                          | `input`을 반환한다.                                          |

- `Symbol.toPrimitive`

  - 목표로 하는 타입(hint)을 명명하는 데 사용된다.

  ```javascript
  obj[Symbol.toPrimitive] = function(hint) {
    // 반드시 원시값을 반환해야 한다.
    // hint는 "string", "number", "default" 중 하나가 될 수 있다.
  };
  ```

  - 실제 예시 코드를 보면서 어떤 느낌이지 파악해보자.

  ```javascript
  const user = {
    name: "John",
    money: 1000,
  
    [Symbol.toPrimitive](hint) {
      alert(`hint: ${hint}`);
      return hint == "string" ? `{name: "${this.name}"}` : this.money;
    }
  };
  
  // 데모:
  alert(user); // hint: string -> {name: "John"}
  alert(+user); // hint: number -> 1000
  alert(user + 500); // hint: default -> 1500
  ```

  - 위 코드와 같이 `Symbol.toPrimitive` 내장 심볼을 이용하여 모든 종류의 형 변환을 다룰 수 있다.

<br>

#### :round_pushpin: `OrdinaryToPrimitive(O, hint)`

- O에 내장된 `toString` 또는 `valueOf` 메서드를 통해 타입이 `Object`인 O을 원시값으로 변환한다. 
- `toString`, `valueOf` 메서드를 통해 원시값으로 변환이 되지 않으면 `TypeError`를 발생한다.

```javascript
// 특정 메서드를 호출 가능한지 판별하는 함수
function IsCallable(x) {
  return typeof x === 'function';
}

function OrdinaryToPrimitive(O: object, hint: 'string' | 'number') {
  let methodNames;
  if (hint === 'string') {
    methodNames = ['toString', 'valueOf'];
  } else {
    methodNames = ['valueOf', 'toString'];
  }
  for (const name of methodNames) {
    const method = O[name];
    if (IsCallable(method)) {
      const result = method.call(O);
      if (TypeOf(result) !== 'object') {
        return result;
      }
    }
  }
  throw new TypeError();
}
```

- 내부 로직 설명(`hint`가 `'string'`인 경우)
  - 참고로 `hint`가 `'number'`인 경우는 아래와 동일하고 `valueOf` => `toString` 순서로 메서드를 확인하면 된다.

| 케이스                                                       | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `O.toString`을 호출할 수 있고 `O.toString.call(O)`의 반환 타입이 `'object'`가 아닌 경우(즉, 원시값인 경우) | 그 결과를 즉시 반환한다.                                     |
| `O.toString`을 호출할 수 있고 `O.toString.call(O)`의 반환 타입이 `'object'`인 경우 | `O.valueOf`를 호출할 수 있는지 확인 후 `O.valueOf.call(O)`의 반환 타입이 `'object'`일 때만 그 결과를 반환한다. |
| `O.valueOf`를 호출할 수 없고 `O.valueOf.call(O)`의 반환 타입이 `'object'`인 경우 | 원시값이 아니므로 `TypeError`를 반환한다.                    |

- `toString`, `valueOf` 메서드

| 메서드     | 설명                  |
| ---------- | --------------------- |
| `toString` | 문자열로 변환해준다.  |
| `valueOf`  | 객체 자신을 반환한다. |

```javascript
// 객체인 경우
const user = {name: "John"};

alert(user); // [object Object]
alert(user.valueOf() === user); // true
```

- 참고로 `ToPrimitive` 함수에서 `Symbol.toPrimitive` 를 직접 만든 예시 코드가 있었는데 이를 아래와 같이 `toString`, `valueOf` 메서드를 조합해서 동일하게 동작하도록 만들 수도 있다.

```javascript
const user = {
  name: "John",
  money: 1000,

  // hint가 "string"인 경우
  toString() {
    return `{name: "${this.name}"}`;
  },

  // hint가 "number"나 "default"인 경우
  valueOf() {
    return this.money;
  }
};

alert(user); // toString -> {name: "John"}
alert(+user); // valueOf -> 1000
alert(user + 500); // valueOf -> 1500
```

<br>

### (2) 숫자로 타입 변환 케이스 내부 연산 따라가기

#### :heavy_check_mark: `Number('1');`

- `Number` 생성자 함수는 `ToNumber(argument)` 연산이 작동된다.
- `'1'`의 type이 `'string'`이므로 `parseTheString` 내부 연산을 거치게 되는데 해당 프로세스는 다소 복잡하여 [ECMAScript 공식 문서](https://tc39.es/ecma262/#sec-runtime-semantics-mv-s)로 대체한다.

<br>

#### :heavy_check_mark: `Number([1]);`

- `ToNumber(argument)`  연산
  - `argument`의 타입이 `object`이므로 `ToPrimitive(input, hint)` 연산으로 넘어간다.
  - 참고로 마지막에  `ToNumber(argument)` 연산 남은 부분이 있어 다시 돌아올 예정이다.
- `ToPrimitive(input, hint)` 연산
  - `input` 값은 `[1]`, `hint`는 주어지지 않으면 `'default'`가 된다.
  - `[1]`의 타입이 `'object'`이고 `[1][Symbol.toPrimitive]`는 `undefined` 이므로 `OrdinaryToPrimitive(O, hint)` 연산으로 넘어간다.
  - 이 때 다음 연산으로 넘어갈 때 `hint`는 `'number'`가 된다.
- `OrdinaryToPrimitive(O, hint)` 연산
  - 현재 호출되는 연산은 `OrdinaryToPrimitive([1], 'number')`이 된다.
  - `hint`가 `'number'`이므로 `valueOf` => `toString` 순서로 해당 메서드를 호출할 수 있는지 확인할 것이다.
  - `valueOf` 메서드의 결과는 `[1]`이고 이 때 타입은 `'object'`이므로 다음 메서드로 넘어간다.
  - `toString` 메서드의 연산 결과는 `Array.prototype.toString()`에 의해 'string' `타입인 `'1'`이 되고 `'object'` 타입이 아니므로 `'1'`을 반환한다.
- `ToNumber(argument)`  연산
  - 이제 마지막으로 `ToNumber('1')` 연산을 실행한다.
  - 이 과정은 처음에 살펴보면 `Number('1');`과 동일하므로 최종 결과는 숫자 `3`이 된다.

<br>

#### :heavy_check_mark: `Number([1, 2]);`

>  `Number([1]);` 예제와 거의 동일하여 `OrdinaryToPrimitive(O, hint)` 연산부터 살펴보도록하자.

- `OrdinaryToPrimitive(O, hint)` 연산
  - 현재 호출되는 연산은 `OrdinaryToPrimitive([1, 2], 'number')`이 된다.
  - `hint`가 `'number'`이므로 `valueOf` => `toString` 순서로 해당 메서드를 호출할 수 있는지 확인할 것이다.
  - `valueOf` 메서드의 결과는 `[1, 2]`이고 이 때 타입은 `'object'`이므로 다음 메서드로 넘어간다.
  - `toString` 메서드의 연산 결과는 `Array.prototype.toString()`에 의해 `'string' `타입인 `'1,2'`이 되고 `'object'` 타입이 아니므로 `'1,2'`을 반환한다.
- `ToNumber(argument)`  연산
  - 이제 마지막으로 `ToNumber('1,2')` 연산을 실행한다.
  - `'1,2'`는 숫자로 변환 가능한 문자열이 아니므로 최종 결과는 `NaN`이 된다.

<br>

---

:bookmark: <b>함께 보면 좋은 자료(feat. Reference)</b>

- 지금은 숫자로 강제 타입 변환할 때만 살펴보았지만 `string`, `boolean` 등 다양한 타입으로 변환하는 경우도 있는데 이는 [여기](https://exploringjs.com/deep-js/ch_type-coercion.html)(Javascript 언어로 작성한 타입 변환 내부 연산)를 참고하면 자세히 알 수 있다.
- 그리고 모던 Javascript 튜토리얼에서도 객체를 원시형으로 변환하는 내용을 [여기](https://ko.javascript.info/object-toprimitive)에 자세히 나와있으니 함께 참고하면 좋을 것 같다.

---

<br>

## 5. 단축 평가

### (1) 논리 연산자를 사용한 단축 평가

(작성중...)
