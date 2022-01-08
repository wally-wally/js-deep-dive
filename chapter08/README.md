# Chapter08. 제어문

<br>

## :hand: Intro

- 일반적으로 코드는 위에서 아래 방향으로 순차적으로 실행되는데 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있다.
- 하지만 제어문을 남발하거나 복잡하게 로직을 구성하는 경우 코드의 흐름을 파악하는데 어려울 수 있어 주의해야 한다.
- 그래서 이러한 문제를 해결하기 위해 최근에는 `Array.prototype.map()`, `Array.prototype.reduce()` 등과 같은 함수형 프로그래밍 기법도 등장했다.

<br>

## **1. 블록문(block statement)**

- 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 한다.
- 문의 끝에는 보통 세미콜론을 붙여 문의 종료를 의미하는데 블록문의 끝에는 세미콜론을 붙이지 않는다.

```jsx
// 블록문
{
  var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  console.log(x--);
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

<br>

## 2. 조건문(conditional statement)

> 주어진 조건식의 평가 결과에 따라 블록문의 실행을 결정한다.
>
> 이때 조건식은 `boolean` 값으로 평가될 수 있는 표현식이다.

### (1) `if ... else` 문

- 조건식의 평가 결과에 따라 실행할 코드 블록을 결정한다.
- 만약 `if` 문의 조건식이 `boolean` 값이 아닌 값으로 평가되면 자바스크립트 엔진에 의해 <b>암묵적으로 `boolean` 값으로 강제 변환</b>되어 실행할 코드 블록을 결정한다.

```jsx
// 일반적인 경우
if (조건식) {
  // 조건식이 참인 경우
} else {
  // 조건식이 거짓인 경우
}

// 여러 조건식을 추가하는 경우
if (조건식1) {
  // 조건식1이 참인 경우
} else if (조건식2) {
  // 조건식2가 참인 경우
} else {
  // 조건식1, 조건식2 모두 거짓인 경우
}

// else if 문과 else 문은 옵션
if (조건식) {
  // 조건식이 참인 경우
}
```

- 만약 블록문 내의 문이 하나라면 중괄호를 생략할 수 있지만 분기 처리가 많아지는 경우 한눈에 파악하기 어려우니 그냥 중괄호를 표기하는 것도 괜찮아보인다.(어떻게 보면 취향차이)

```jsx
var num = 2;

if (num > 0) return '양수';
else if (num < 0) return '음수';
else return 'zero';
```

- 대부분의 `if ... else` 문은 삼항 조건 연산자로 바꿔 쓸 수 있다.

```jsx
// if ... else 문을 사용한 경우
function check(x) {
  if (x % 2) {
    return '홀수';
  } else {
    return '짝수';
  }
}

// 삼항 조건 연산자를 사용한 경우
function check(x) {
  return x % 2 ? '홀수' : '짝수';
}
```

- 만약 분기 처리가 두 가지 케이스가 아닌 여러 케이스일 때 삼항 조건 연산자는 아래와 같이 그룹 연산자를 함께 사용해서 작성할 수 있다.
  - 아래 코드에서 `num > 0 ? '양수' : '음수'` 는 표현식으로 삼항 조건 연산자는 값으로 평가되는 표현식을 만든다.
  - 따라서 값처럼 사용할 수 있어 변수에 할당할 수 있고 반면 `if ... else` 문은 값처럼 사용할 수 없기 때문에 변수에 할당할 수 없다.

```jsx
function check(num) {
  if (num > 0) return '양수';
  else if (num < 0) return '음수';
  else return 'zero';
}

function check(num) {
  return num ? (num > 0 ? '양수' : '음수') : 'zero';
}
```

- 삼항 조건 연산자가 간결하게 코드를 작성할 수 있어서 좋지만 무조건 삼항 조건 연산자가 모든 상황에서 좋지는 않다.
  - 조건에 따라 단순히 값을 결정하여 변수에 할당하는 경우 삼항 조건 연산자 방식이 좋다.
  - 반면 조건에 따라 실행해야 할 내용이 복잡하거나 분기 처리해야하는 조건의 개수가 많다면 `if ... else` 문이 더 적합하다.

<br>

### (2) `switch` 문

- 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 `case` 문으로 실행 흐름을 옮긴다.
- `switch` 문의 표현식과 일치하는 `case` 문이 없다면 실행 순서는 `default` 문으로 이동한다.
  - 참고로 `default` 문은 선택사항으로, 사용할 수도 있고 사용하지 않을 수도 있다.

```jsx
switch (표현식) {
  case 표현식1:
    // switch 문의 표현식과 표현식1이 일치하면 실행될 문;
    break;
  case 표현식2:
    // switch 문의 표현식과 표현식2가 일치하면 실행될 문;
    break;
  default:
    // switch 문의 표현식과 일치하는 case 문이 없을 때 실행될 문;
}
```

- `if ... else` 문의 조건식은 `boolean` 값으로 평가되어야 하지만 `switch` 문의 표현식은 `boolean` 값보다는 문자열이나 숫자 값인 경우가 많다.

  - `switch` 문은 논리적 참, 거짓보다는 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용한다.

- fall through

  ```jsx
  var num = 3;
  var result;
  
  switch (num) {
    case 1: result = '1';
    case 2: result = '2';
    case 3: result = '3';
    default: result = 'default';
  }
  
  console.log(result); // 'default'
  ```

  - 위 코드를 보면 `'3'`이 출력될 것으로 예상했지만 `'default'`가 출력되었다.
  - 이는 `switch` 문이 끝날 때까지 이후의 모든 `case` 문과 `default` 문을 실행했기 때문인데 이를 fall through라고 한다.
  - 그래서 이러한 경우를 방지하기 위해 `break` 문을 사용해야 한다.
  - `break` 문이 없다면 `case` 문의 표현식과 일치하지 않더라도 실행 흐름이 다음 `case` 문으로 연이어 이동한다.

  ```jsx
  var num = 3;
  var result;
  
  switch (num) {
    case 1: result = '1';
      break;
    case 2: result = '2';
      break;
    case 3: result = '3';
      break;
    default: result = 'default'; // default 문에는 break 문을 생략하는 것이 일반적이다.
  }
  
  console.log(result); // '3'
  ```

  - 참고로 `break` 문을 생략하면 더 유용한 경우도 있다. 아래 예시를 참고해보자.

  ```jsx
  var year = 2020;
  var month = 2;
  var days = 0;
  
  switch (month) {
    case 1: case 3: case 5: case 7: case 8: case 10: case 12:
      days = 31;
      break;
    case 4: case 6: case 9: case 11:
      days = 30;
      break;
    case 2:
      days = ((year % 4 === 0 && year % 1000 !== 0) || (year % 400 === 0)) ? 29: 28;
      break;
    default:
      console.log('invalid month');
  }
  
  console.log(days); // 29
  ```

- 만약 `if .. else` 문으로 해결할 수 있다면 `switch` 문보다 `if ... else` 문을 사용하는 편이 좋다.

  - 하지만 조건이 너무 많아서 `if ... else` 문보다 `switch` 문을 사용했을 때 가독성이 좋다면 `switch` 문을 사용하는 편이 좋다.
  - **상황에 따라서 적절한 조건문을 사용하자.**

<br>

## 3. 반복문(loop statement)

> 반복문은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다.
>
> 그 후 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행한다.
>
> 이는 조건식이 거짓일 때까지 반복된다.

### (1) `for` 문

```jsx
for (변수 선언문 또는 할당문; 조건식; 증감식) {
  조건식이 참인 경우 반복 실행될 문;
}
for (var i = 0; i < 3; i += 1) {
  console.log(i);
}

// 출력 결과
0
1
2
```

- 참고로 `for` 문의 변수 선언문, 조건식, 증감식은 모두 옵션이므로 반드시 사용할 필요는 없다.
  - 단, 아래 코드와 같이 어떤 식도 선언하지 않으면 무한 루프가 된다.

```jsx
// 무한 루프
for (;;) { ... }
```

- `for` 문 내에 `for` 문을 중첩해 사용할 수 있는데 이를 **중첩 `for` 문**이라 한다.

```jsx
for (var i = 0; i < 3; i += 1) {
  for (var j = 0; j < 3; j += 1) {
    console.log(i, j);
  }
}

// 출력 결과
0 0
0 1
0 2
1 0
1 1
1 2
2 0
2 1
2 2
```

<br>

### (2) `while` 문

- 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다.
  - 조건식의 평가 결과가 거짓이 되면 코드 블록을 실행하지 않고 종료한다.
- <b>`for` 문은 반복 횟수가 명확할 때 주로 사용하고 `while` 문은 반복 횟수가 불명확할 때 주로 사용한다.</b>

```jsx
var count = 0;

while (count < 3) {
  console.log(count); // 0 1 2
  count += 1;
}
```

- 만약 조건식의 평가 결과가 `true` 로 고정되면 무한루프가 된다.
  - 무한루프에서 탈출하기 위해서는 코드 블록 내에서 탈출 조건 구문을 작성해야한다.

```jsx
var count = 0;

while (true) {
  console.log(count);
  count += 1;

  if (count === 3) {
    break;
  }
}

// 출력 결과
0
1
2
```

<br>

### (3) `do ... while` 문

- `while` 문과 달리 코드 블록을 먼저 실행하고 조건식을 평가한다.
  - 따라서 코드 블록은 무조건 한 번 이상 실행된다.

```jsx
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
do {
  console.log(count); // 0 1 2
  count += 1;
} while (count < 3);
```

<br>

## 4. `break` 문

- `break` 문은 코드 블록을 탈출하는 역할을 한다.
  - 좀 더 정확히 표현하자면 코드 블록을 탈출하는 것이 아니라 레이블 문, 반복문 또는 `switch` 문의 코드 블록을 탈출한다.
  - 레이블 문은 `6. label 문` 절에서 좀 더 자세히 살펴보자.
- 레이블 문, 반복문, `switch` 문의 코드 블록 외에 `break` 문을 사용하면 `SyntaxError` 가 발생한다.

```jsx
if (true) {
  break;
}
```

<br>

### (1) `break` 문 사용 예시 - `for` 문

```jsx
var usernames = ['wally', 'mory', 'henry', 'tigger', 'chang'];
var targetUsername = 'henry';
var index = -1;

for (var i = 0; i < usernames.length; i += 1) {
  if (usernames[i] === targetUsername) {
    index = i;
    break;
  }
} 

console.log(index); // 2
```

```javascript
// 참고) 개선된 refactoring 코드
// Array.prototype.findIndex()와 관련된 내용은 추후에 다시 알아보자.

const usernames = ['wally', 'mory', 'henry', 'tigger', 'chang'];
const targetUsername = 'henry';

const index = usernames.findIndex((username) => username === targetUsername);

console.log(index); // 2
```

<br>

### (2)  `break` 문 사용 예시 - `switch` 문

```jsx
let nextOrder = targetRule.order;
switch (action) {
  case 'TOP':
    nextOrder = Math.max(1, nextOrder - 1);
    break;
  case 'TOP-MOST':
    nextOrder = 1;
    break;
  case 'BOTTOM':
    nextOrder = Math.min(this.receiveRules.length, nextOrder + 1);
    break;
  case 'BOTTOM-MOST':
    nextOrder = this.receiveRules.length;
    break;
  default:
    break;
}
```

<br>

## 5. `continue` 문

- 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다.
- `break` 문처럼 반복문을 탈출하지는 않는다.

```jsx
var usernames = ['wally', 'mory', 'henry', 'tigger', 'chang'];
var targetAlphabet = 'y';
var count = 0; // 알파벳 'y'가 포함된 이름의 총 개수

for (var i = 0; i < usernames.length; i += 1) {
  if (usernames[i].indexOf(targetAlphabet) === -1) {
    continue;
  }

  count += 1;
} 

console.log(count); // 3
```

------

### ➕ `continue` 문 대신에 `if` 문을... - `no-continue`

- 자바스크립트 언어로 애플리케이션을 개발하면서 `continue` 문을 사용하는 코드가 있을 때 eslint 옵션을 킨 경우 아래와 같은 error를 볼 수 있다.

![0000](https://user-images.githubusercontent.com/52685250/148641191-dbabf121-9b54-4630-9a81-84d57229b47f.png)

- eslint는 `continue` 문을 반기지 않는다.
  - eslint에서는 잘못 사용하면 코드의 테스트 가능성이 떨어지고 읽기 어려우며 유지 보수하는데 용이하지 않는다고 말하고 있다.
  - `continue` 문 대신에 `if` 문과 같은 제어문을 사용하라고 권장하고 있다.
  - 상세 내용은 [여기](https://eslint.org/docs/rules/no-continue)서 확인할 수 있다.

```javascript
var a = 0;

// Bad
for(i = 0; i < 10; i++) {
  if (i >= 5) {
    continue;
  }

  a += i;
}

// Good
for(i = 0; i < 10; i++) {
  if (i >= 5) {
    a += i;
  }
}
```

------

<br>

## 6. `label` 문

- 레이블 문은 프로그램의 실행 순서를 제어하는 데 사용한다.
  - 참고로 `switch` 문의 `case` 문과 `default` 문도 레이블 문이다.
  - 레이블 문에 대한 상세 내용은 [MDN 공식 문서 - label문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/label)에서 확인하자.
- 레이블 문을 탈출하려면 `break` 문에 레이블 식별자를 지정한다.

```jsx
foo: {
  console.log(1);
  break foo;
  console.log(2);
}

console.log('Done!');

// 출력 결과
1
Done!
```

```javascript
outer: for (var i = 0; i < 3; i += 1) {
  inner: for (var j = 0; j < 3; j += 1) {
    if (i + j === 3) {
      break outer;
    }

    console.log(`inner: [${i}, ${j}]`);
  }
}

console.log('Done!');

// 출력 결과
inner: [0, 0]
inner: [0, 1]
inner: [0, 2]
inner: [1, 0]
inner: [1, 1]
Done!
```

- `continue` 문에서도 레이블 식별자를 지정할 수 있다.

```javascript
outer: for (var i = 0; i < 3; i += 1) {
  inner: for (var j = 0; j < 3; j += 1) {
    if (i + j === 3) {
      continue outer;
    }

    console.log(`inner: [${i}, ${j}]`);
  }
}

console.log('Done!');

// 출력 결과
inner: [0, 0]
inner: [0, 1]
inner: [0, 2]
inner: [1, 0]
inner: [1, 1]
inner: [2, 0]
Done!
```

- 레이블 문은 중첩된 `for` 문 외부로 탈출할 때 유용하지만 그 밖의 경우에는 <b>일반적으로 권장하지 않는다.</b>
  - 레이블 문을 사용하면 프로그램의 흐름이 복잡해져서 가독성이 나빠지고 오류를 발생시킬 가능성이 높아지기 때문이다.
