# Chapter06. 데이터 타입

<br>

## :hand: Intro - 데이터 타입의 종류

| 타입                 | 종류                                             |
| -------------------- | ------------------------------------------------ |
| 원시 타입            | number, string, boolean, null, undefined, symbol |
| 객체 타입(참조 타입) | 객체, 함수, 배열 등                              |

<br>

## 1. number

- 배정밀도 64비트 부동소수점 형식을 따른다.
- 모든 수를 실수로 처리하며, 정수만 표현하기 위한 데이터 타입이 별도로 존재하지 않는다.
  - 그렇기 때문에 정수로 표시된다 해도 사실은 실수라는 것을 의미한다.
  - 그리고 정수로 표시되는 수끼리 나누더라도 실수가 나올 수 있다.

```javascript
console.log(1 === 1.0); // true
console.log(6 / 2); // 3
console.log(3 / 2); // 1.5
```

- 2진수, 8진수, 16진수를 표현하기 위한 데이터 타입도 제공하지 않으므로 이들 값을 참조하면 모두 10진수로 해석된다.

```javascript
var binary = 0b01000001; // 2진수
var octal = 0o101; // 8진수
var hex = 0x41; // 16진수

console.log(binary); // 65
console.log(octal); // 65
console.log(hex); // 65
console.log(binary === octal); // true
console.log(octal === hex); // true
```

- `number` 타입은 `Infinity`, `-Infinity`, `NaN` 과 같은 특별한 값도 표현할 수 있다.

```javascript
console.log(100 / 0); // Infinity
console.log(100 / -0); // -Infinity
console.log(12 - 'hello'); // NaN
console.log(Infinity / 0); // Infinity
console.log(Infinity / Infinity); // NaN
```

---

### :heavy_plus_sign: `isNaN` vs `Number.isNaN`

#### :round_pushpin: `isNaN`

- `isNaN`은 숫자인지 아닌지 검사하는 함수이다.
- `true`인 경우 숫자가 아니고 `false`인 경우 숫자이다.

```javascript
isNaN(1); // false
isNaN('1'); // false
isNaN('wow'); // true
isNaN(undefined); // true
```

- 하지만 `null`은 우리가 예상한대로 `true`가 아니라 `false` 결과를 return하게 된다.

```javascript
isNaN(null); // false
```

- 그 이유는 `isNaN()` 함수는 인자로 넘어오는 값을 `Number` 생성자를 이용해서 타입 변환을 시도하게 된다.
  - 그렇기 때문에 위의 예시에서 `isNaN('1');` 의 return 값이 `false` 이었던 이유가 이 때문이다.
  - `Number(null)` 의 결과값은 `0`이여서 `isNaN(0)`에 의해 `false`를 출력하는 것이다.

#### :round_pushpin: `Number.isNaN`

- 이러한 문제를 해결하기 위해 ES2015에서 `Number.isNaN()` 메서드가 추가되었다.
- 이 메서드는 `isNaN()` 과 달리 강제로 타입 변환을 시도하지 않는다.
  - 인자 값의 타입이 `number`이고, `NaN`이면 `true`, 아니면 `false`를 출력하게 된다.
- 그로 인해 `isNaN`에서 예상과 다르게 return되는 것들이 이제는 올바르게 return되는 것을 볼 수 있다.

```javascript
Number.isNaN(1); // false
Number.isNaN('1'); // false
Number.isNaN('wow'); // false
Number.isNaN(undefined); // false
Number.isNaN(null); // false

Number.isNaN(NaN); // true
Number.isNaN(Number.NaN); // true
Number.isNaN(0 / 0); // true
```

- 그리고 해당 메서드는 Internet Explorer 브라우저에서는 제공하지 않으니 IE에서도 이 메서드를 사용하려면 폴리필을 반드시 추가해야 한다.

---

### :heavy_plus_sign: `Number.MAX_SAFE_INTEGER`

- 보통 최솟값을 구하는 알고리즘 문제를 풀 때 초기에 정답을 담은 변수에 임시로 최댓값을 설정한 후 알고리즘을 풀기 시작한다.
  - 이 때 초기 최댓값을 보통 무한대와 관련된 값들을 설정하곤 하는데 자바스크립트 언어에서는 양의 무한대를 의미하는 `Infinity`와 같은 값을 설정하면 안 된다.
- 자바스크립트 언어에서 안전하게 표현할 수 있는 최대 정수 값인 `Number.MAX_SAFE_INTEGER` 사용하는 것을 권장한다.
- 아래 코드에서 보는 것과 같이 `Number.MAX_SAFE_INTEGER` (=== 2<sup>53</sup> - 1)에서 1씩 증가하여 더할 때마다 결과 값이 비정상적으로 증가하는 모습을 볼 수 있다.
  - 이렇게 출력되는 이유는 자바스크립트에서 number 타입이 배정밀도 64비트 부동소수점 형식을 따르고 있기 때문이다.

```javascript
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
console.log(Number.MAX_SAFE_INTEGER + 1); // 9007199254740992
console.log(Number.MAX_SAFE_INTEGER + 2); // 9007199254740992
console.log(Number.MAX_SAFE_INTEGER + 3); // 9007199254740994
console.log(Number.MAX_SAFE_INTEGER + 4); // 9007199254740996
console.log(Number.MAX_SAFE_INTEGER + 5); // 9007199254740996
```

- 만일 어떤 수가 안전한 정수로 표현된 것인지 확인하려면 `Number.isSafeInteger()` 메서드를 사용하면 된다.

```javascript
Number.isSafeInteger(3);                    // true
Number.isSafeInteger(Math.pow(2, 53));      // false
Number.isSafeInteger(Math.pow(2, 53) - 1);  // true
Number.isSafeInteger(NaN);                  // false
Number.isSafeInteger(Infinity);             // false
Number.isSafeInteger('3');                  // false
Number.isSafeInteger(3.1);                  // false
Number.isSafeInteger(3.0);                  // true

// 예제 출처: Number.isSafeInteger() MDN 공식 문서
```

- 그리고 해당 상수는 Internet Explorer 브라우저에서는 제공하지 않으니 IE에서도 이 메서드를 사용하려면 폴리필을 반드시 추가해야 한다.

---

<br>

## 2. string

- 0개 이상의 16비트 유니코드 문자열(UTF-16)의 집합으로 전 세계 대부분의 문자를 표현할 수 있다.
- 작은따옴표, 큰따옴표, backtick으로 텍스트를 감싸서 표현한다.

```javascript
var str1 = 'hello';
var str2 = "hello";
var str3 = `hello`;
```

<br>

## 3. Template Literal

> 일반 문자열과 비슷해 보이지만 작은따옴표 또는 큰따옴표 같은 일반적인 따옴표 대신 backtick을 사용해서 표현하는 문자열 표기법이다.

### (1) 멀티라인 문자열

- 일반 문자열과 다르게 줄바꿈(개행)이 허용된다.
- `console.log`로 해당 값을 확인하면 줄바꿈된 부분은 이스케이프 시퀀스를 사용해서 `\n`와 같이 표현된다.

```javascript
var str = `hello!
Nice to Meet You!
   Bye!
`;

console.log(str); // 'hello!\nNice to Meet You!\n   Bye!\n'
```

- 이스케이프 시퀀스(escape sequence)

| 이스케이프 시퀀스 | 의미                                                         |
| ----------------- | ------------------------------------------------------------ |
| `\n`              | 개행(LF; Line Feed): 다음 행으로 이동                        |
| `\r`              | 개행(CR; Carriage Return): 커서를 처음으로 이동              |
| `\f`              | 폼 피드(Form Feed): 프린터로 출력할 경우 다음 페이지의 시작 지점으로 이동 |
| `\t`              | 탭(수평)                                                     |
| `\v`              | 탭(수직)                                                     |
| `\b`              | 백스페이스                                                   |

<br>

### (2) 표현식 삽입

- 문자열은 문자열 연산자 `+`를 사용해서 연결할 수 있는데 연결하는게 많아지는 경우 가독성도 떨어지고 문자열을 연결하는데 불편한 단점이 있다.
- 그래서 이를 해결하기 위해 `${}`으로 표현식을 감싸서 표현식을 삽입할 수 있다.

```javascript
// variables
var name = 'wally';
var age = 29;

// before
console.log('My name is ' + name + '. My age is ' + age + '.'); // My name is wally. My age is 29.

// after
console.log(`My name is ${name}. My age is ${age}.`); // My name is wally. My age is 29.
```

- 또한 `${}` 안의 표현식의 평가 결과가 문자열이 아니더라도 문자열로 타입이 강제로 변환되어 삽입된다.

```javascript
console.log(`1 + 2 = ${1 + 2}`); // 1 + 2 = 3
```

---

### :heavy_plus_sign: 평가와 일급

- 평가 : 코드가 계산(evaluation) 되어 값을 만드는 것

- 일급

  - 값으로 다룰 수 있다.
  - 변수에 담을 수 있다.
  - 함수의 인자로 사용될 수 있다.
  - 함수의 결과로 사용될 수 있다.

  ```javascript
  const a = 10;
  const add10 = a => a + 10;
  const r = add10(a);
  log(r);
  ```

- 일급 함수

  - 함수를 값으로 다룰 수 있다.

  ```javascript
  const add5 = a => a + 5;
  log(add5); // a => a + 5
  log(add5(5)); // 10
  ```

  - 조합성과 추상화의 도구

  ```javascript
  const f1 = () => () => 1;
  log(f1()); // () => 1 (함수가 출력)
  
  const f2 = f1();
  log(f2); // () => 1
  log(f2()); // 1
  ```

---

<br>

## 4. boolean

- 논리적 참, 거짓을 나타내는 `true`와 `false` 뿐이다.

<br>

## 5. undefined vs null

> Javascript 에서는 두 가지 방법으로 값의 부재를 표현한다.

### (1) undefined

- `var` 키워드로 선언한 변수는 초기화하지 않으면 암묵적으로 `undefined` 값을 가지게 된다.
- 즉, 개발자가 의도적으로 할당하기 위한 값이 아니라 자바스크립트 엔진이 변수를 초기화할 때 사용하는 값이다.

<br>

### (2) null

- `undefined`와 달리 `null`은 의도적인 값의 부재를 의미한다.
- 변수에 `null`을 할당하는 것은 변수가 이전에 참조하던 값을 더 이상 참조하지 않겠다는 의미다.
- 이는 이전에 할당되어 있던 값에 대한 참조를 명시적으로 제거하는 것을 의미하며, 자바스크립트 엔진은 누구도 참조하지 않는 메모리 공간에 대해 가비지 컬렉션을 수행할 것이다.

---

### :heavy_plus_sign: DOM 요소 타입 정의(Typescript)

- 일반적으로 `document.querySelector()` 를 이용해서 DOM 요소를 가져오게 되면 타입은 자동적으로 `Element | null` 로 추론된다.
  - 왜냐하면 코드 입장에서는 실제 DOM 요소가 존재할 수도 있고 존재하지 않을 수도 있다고 생각하기 때문이다.
- 아래 코드에서 `froalaEditorElem`은 iframe 요소인데 해당 요소 안에 있는 `contentDocument`에 접근하려고 하면 참조할 수 없다는 에러 표시가 뜨게된다.

![001](https://user-images.githubusercontent.com/52685250/147869781-0650982d-263c-4ae7-a6ed-ba913a6c49e3.png)

- 그래서 이러한 경우 타입스크립트에서는 `as` 키워드를 활용한 타입 단언을 이용해서 해당 DOM 요소의 명확한 타입을 명시하여 해결할 수 있다.

![002](https://user-images.githubusercontent.com/52685250/147869829-8e897439-9665-4d3a-9cda-7384bb2a5e00.png)

---

<br>

## 6. symbol

- ES2015에서 추가된 7번째 타입으로, 변경 불가능한 원시 타입의 값이다.
- 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다.
- 따라서 주로 이름이 충돌할 위험이 없는 객체의 유일한 프로퍼티 키를 만들기 위해 사용한다.
- `Symbol` 함수를 호출해 생성할 수 있으며 이 때 생성된 심벌 값은 외부에 노출되지 않고 다른 값과 절대 중복되지 않는 유일무이한 값이다.

```javascript
var s1 = Symbol('key1');
console.log(typeof s1); // symbol
```

<br>

## 7. 객체 타입(참조 타입)

- 지금까지 다룬 원시 타입들을 제외한 나머지들이 모두 객체 타입이다.
- 자바스크립트에서 이루고 있는 거의 모든 것이 객체이다.
- 추후 원시 값과 객체에 대해서 자세히 비교해보는 시간을 가져보자.

<br>

## 8. 데이터 타입의 필요성

### (1) 데이터 타입에 의한 메모리 공간의 확보와 참조

- 값은 메모리에 저장하고 참조할 수 있어야 한다.
- 메모리에 값을 저장하려면 먼저 확보해야 할 <b>메모리 공간의 크기를 결정</b>해야 한다.
  - 그래야 메모리 공간의 낭비와 손실 없이 값을 저장할 수 있다.
- 자바스크립트 엔진은 데이터 타입, 즉 값의 종류에 따라 정해진 크기의 메모리 공간을 확보한다.
  - 즉, 변수에 할당되는 값의 데이터 타입에 따라 확보해야 할 메모리 공간의 크기가 결정된다.
- 값을 참조하려면 한 번에 읽어 들여야 할 메모리 공간의 크기, 즉 메모리 셀의 개수(바이트 수)를 알아야 한다
- 컴퓨터는 해당 변수에 할당된 값의 타입에 따라 몇 바이트 단위로 메모리 공간에 저장된 값을 읽어 들이는지 결정한다.

<br>

### (2) 데이터 타입에 의한 값의 해석

- 모든 값은 데이터 타입을 가지며, 메모리에 2진수, 즉 비트의 나열로 저장된다.
- 메모리에 저장된 값은 데이터 타입에 따라 다르게 해석될 수 있다.
- 예를 들어, 메모리에 저장된 값 `0100 0001`을 숫자로 해석하면 `65`지만 문자열로 해석하면 `A`다.
- 즉, 메모리에서 읽어 들인 <b>2진수를 어떻게 해석할지 결정</b>하기 위해 데이터 타입이 필요한 것이다.

<br>

## 9. 동적 타이핑

### (1) 동적 타입 언어, 정적 타입 언어

- 정적 타입 언어는 변수를 선언할 때 변수에 할당할 수 있는 값의 종류, 즉 데이터 타입을 사전에 선언해야 한다.
  - 변수의 타입을 변경할 수 없으며, 변수에 선언한 타입에 맞는 값만 할당할 수 있다.
- 자바스크립트는 정적 타입 언어와 다르게 변수를 선언할 때 타입을 선언하지 않는다.
  - 미리 선언한 데이터 타입의 값만 할당할 수 있는 것이 아니라 어떠한 데이터 타입의 값이라도 자유롭게 할당할 수 있다.
  - 이러한 언어를 동적 타입 언어라고 한다.

```javascript
var foo;
console.log(typeof foo); // undefined

foo = 123;
console.log(typeof foo); // number

foo = true;
console.log(typeof foo); // boolean
```

- 자바스크립트의 변수는 선언이 아닌 할당에 의해 타입이 결정(타입 추론)된다.
  - 그리고 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있는데 이러한 특징을 동적 타이핑이라고 한다.
  - 동적 타입 언어로는 자바스크립트, 파이썬, PHP, 루비 등이 있다.

<br>

### (2) 동적 타입 언어와 변수

- 자바스크립트는 개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동으로 변환되기도 한다.
  - 즉, 숫자 타입의 변수일 것이라고 예측했지만 사실은 문자열 타입의 변수일 수도 있다는 말이다.
  - 결국 동적 타입 언어는 유연성은 높지만 신뢰성은 떨어진다.
  - 이러한 문제를 해결하기 위해 타입스크립트라는 정적 타입 언어가 등장했다.
- 변수 사용시 주의사항
  - 변수는 꼭 필요한 경우에 한해 제한적으로 사용
  - 변수의 유효 범위(스코프)를 최소화
  - 전역 변수는 최대한 사용하지 않기
  - 변수보다는 상수를 사용해 값의 변경을 억제
  - 의미 있는 변수 네이밍
- 코드는 오해하지 않도록 작성해야 한다.
  - 오해를 유발하는 코드는 개발의 생산성을 매우 떨어뜨린 부분 중 하나이다.
  - 따라서 사람이 이해할 수 있는 코드, 즉 가독성이 좋은 코드가 좋은 코드다.