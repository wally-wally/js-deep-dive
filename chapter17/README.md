# Chapter17. 생성자 함수에 의한 객체 생성

<br>

## 1. `Object` 생성자 함수

- `new` 연산자와 함께 `Object` 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다.
  - 생성자 함수란 `new` 연산자와 함께 호출하여 <b>객체(인스턴스)를 생성하는 함수</b>를 말한다.
  - 생성자 함수에 의해 생성된 객체를 <b>인스턴스</b>라 한다.
- 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

```javascript
// 빈 객체 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'wally';
person.sayHello = function() {
  console.log(`Hello, ${this.name}`);
}

person.sayHello(); // 'Hello, wally';
```

- 반드시 `Object` 생성자 함수를 사용해 빈 객체를 생성해야 하는 것은 아니다.
  - 객체를 생성하는 방법은 <b>객체 리터럴을 사용하는 것이 더 간편</b>하다.
  - `Object` 생성자 함수를 사용해 객체를 생성하는 방식은 특별한 이유가 없다면 그다지 유용해 보이지 않는다.

---

### :heavy_plus_sign: 내장 객체(빌트인 생성자 함수)

- 자바스크립트는 다음과 같은 내장 객체(빌트인 생성자 함수)를 제공한다.
  - `String`, `Number`, `Boolean`, `Object`, `Function`, `Array`, `Date`, `RegExp`, `Error`, `Promise` 등
- 이 내장 객체는 타입 처럼 보이는 데다 클래스처럼 생겼지만 단지 자바스크립트의 내장 함수일 뿐 각각 생성자로 사용되어 주어진 하위 타입의 객체를 생성한다.

```javascript
var str1 = 'hello'; // str1은 원시 래퍼 객체가 아닌 원시 리터럴이며 불변값
console.log(typeof str1); // 'string'
console.log(str1 instanceof String); // false

var str2 = new String('hello');
console.log(typeof str2); // 'object'
console.log(str2 instanceof String); // true
```

- 여러 자바스크립트 커뮤니티에서는 되도록 생성자 형식은 지양하고 리터럴 형식을 사용하라고 적극 권장한다.
- 특정 원시 값에 대해 프로퍼티/메서드를 호출하면 자바스크립트 엔진은 원시 값을 자동으로 원시 래퍼 객체로 강제변환하여 메서드 접근이 가능하게 도와준다.
- `null`, `undefined`는 원시 래퍼 객체가 존재하지 않고, `Date` 값은 리터럴 형식이 없어 반드시 생성자 형식으로 생성해야 한다.
- `Object`, `Function`, `Array`, `RegExp`는 형식과 무관하게 모두 객체다.
  - 추가 옵션이 필요한 경우에만 생성자 형식을 이용하고 일반적으로는 리터럴 방식을 이용하자.

```javascript
// 생성자 형식
const reg1 = new RegExp(/[0-9]/, 'g');

// 리터럴 형식
const reg2 = /[0-9]/g;
```

- `Error`는 생성자 형식으로 생성은 되지만 거의 쓸 일이 없다. 그냥 리터럴 방식만 기억하자.
- 원시 타입과 래퍼 객체는 거의 동등한 값처럼 다뤄져서 아래와 같은 결과가 나온다.
  - [참고 문서] - [Object.prototype.valueOf() - MDN 공식 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf)

```javascript
var n = 1;
var N = new Number(1);

console.log(n == N); // true
console.log(n === N); // false
console.log(n === N.valueOf()); // true
```

---

<br>

## 2. 생성자 함수

### (1) 객체 리터럴에 의한 객체 생성 방식의 문제점

- 객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편한다.
- 하지만 이 방식은 단 하나의 객체만 생성한다.
- 따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.

```javascript
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};

const circle1 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  }
};
```

- 위 코드와 같이 객체의 프로퍼티 구조가 유사한 경우가 많이 있다.
  - 객체 고유의 상태 데이터인 `radius` 프로퍼티의 값은 객체마다 다를 수 있지만 `getDiameter` 메서드는 완전히 동일하다.

<br>

### (2) 생성자 함수에 의한 객체 생성 방식의 장점

- 생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

- 만약 `new` 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작하고 `new` 연산자와 함께 호출해야 해당 함수는 생성자 함수로 동작한다.

```javascript
const circle3 = Circle(15);

console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle 내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

<br>

### (3) 생성자 함수의 인스턴스 생성 과정

#### :round_pushpin: 인스턴스 생성과 `this` 바인딩

- 암묵적으로 빈 객체가 생성된다.
  - 이 빈 객체가 바로 생성자 함수가 생성한 인스턴스다.
- 그리고 암묵적으로 생성된 빈 객체, 즉 인스턴스는 `this`에 바인딩된다.
  - 생성자 함수 내부의 `this`가 생성자 함수가 생성할 인스턴스를 가리키는 이유가 바로 이것이다.
- 이 처리는 함수 몸체의 코드가 한 줄씩 실행되는 런타임 이전에 실행된다.

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
  console.log(this); // Circle {}

  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}
```

<br>

#### :round_pushpin: 인스턴스 초기화

- 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 `this`에 바인딩되어 있는 인스턴스를 초기화한다.(개발자가 기술하는 부분)

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}
```

<br>

#### :round_pushpin: 인스턴스 반환

- 생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.

```javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };

  // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: f}
```

- 만약 `this`가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 `return` 문에 명시된 객체가 반환된다.

```javascript
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
  return {};
}

const circle = new Circle(1);
console.log(circle); // {}
```

- 하지만 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 `this`가 반환된다.

```javascript
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
  return 1;
}

const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: f}
```

- 이처럼 생성자 함수 내부에서 명시적으로 `this`가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손한다.
  - 따라서 생성자 함수 내부에서 `return` 문을 반드시 생략해야 한다.

<br>

### (4) 내부 메서드 `[[Call]]`과 `[[Construct]]`

- 함수는 객체이므로 일반 객체와 동일하게 동작할 수 있다.
  - 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문이다.

```javascript
// 함수는 객체다.
function foo() {}

// 함수는 객체이므로 프로퍼티를 소유할 수 있다.
foo.prop 10;

// 함수는 객체이므로 메서드를 소유할 수 있다.
foo.method = function() {
  console.log(this.prop);
}

foo.method(); // 10
```

- 함수는 객체이지만 일반 객체와는 다르게 <b>일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.</b>
  - 따라서 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위해 함수 객체만을 위한 `[[Environment]]`, `[[FormalParameters]]` 등의 내부 슬롯과 `[[Call]]` , `[[Construct]]` 같은 내부 메서드를 추가로 가지고 있다.
  - (참고 문서) - [Formal parameter, Actual parameter](https://yunmap.tistory.com/entry/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%96%B8%EC%96%B4-Formal-parameter-Actual-parameter-%EA%B7%B8%EB%A6%AC%EA%B3%A0-parameter-passing)
- 함수가 <u>일반 함수로서 호출</u>되면 함수 객체의 내부 메서드 <b>`[[Call]]`</b> 이 호출되고 new 연산자와 함께 <u>생성자 함수로서 호출</u>되면 내부 메서드 <b>`[[Construct]]`</b> 가 호출된다. 

```javascript
// 함수는 객체다.
function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출
new foo();
```

- 용어 정의

| 용어            | 의미                                                         |
| --------------- | ------------------------------------------------------------ |
| callable        | 내부 메서드 `[[Call]]` 을 갖는 함수 객체로 호출할 수 있는 객체, 즉 함수를 말한다. |
| constructor     | 내부 메서드 `[[Construct]]` 를 갖는 함수 객체로 생성자 함수로서 호출할 수 있는 함수를 말한다. |
| non-constructor | 내부 메서드 `[[Construct]]` 를 갖지 않는 함수 객체로 생성자 함수로서 호출할 수 없는 함수를 말한다. |

- 호출할 수 없는 객체는 함수 객체가 아니므로 함수로서 가능한 객체, 즉 함수 객체는 반드시 callable이어야 한다.
  - 따라서 모든 함수 객체는 내부 메서드 `[[Call]]`을 갖고 있으므로 호출할 수 있다.
- 결론적으로 모든 함수 객체는 callable이지만 모든 함수 객체가 constructor인 것은 아니다.

<br>

### (5) constructor와 non-constructor의 구분

- 구분

| 용어            | 의미                                                         |
| --------------- | ------------------------------------------------------------ |
| constructor     | 일반 함수 또는 생성자 함수로서 호출할 수 있는 객체로 함수 선언문, 함수 표현식, 클래스(참고로 클래스도 함수임)가 해당된다. |
| non-constructor | 일반 함수로서만 호출할 수 있는 객체로 메서드(<u>ES6 메서드 축약 표현</u>), 화살표 함수가 해당된다. |

- non-constructor인 함수 객체는 내부 메서드 `[[Construct]]`를 갖지 않는다.
  - 따라서 non-constructor인 함수 객체를 생성자 함수로서 호출하면 에러가 발생한다.

```javascript
function foo() {}

foo(); // 에러 없음
new foo(); // 에러 없음
```

```javascript
const obj = {
  a() {}
};

obj.a(); // 에러 없음
new obj.a(); // Uncaught TypeError: obj.a is not a constructor
```

```javascript
const bar = () => {};

bar(); // 에러 없음
new bar(); // Uncaught TypeError: bar is not a constructor
```

- 주의할 것은 생성자 함수로서 호출될 것을 기대하고 정의하지 않은 일반 함수(callable이면서 constructor)에 `new` 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수 있다는 것이다.

<br>

### (6) `new` 연산자

- `new` 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다.
  - 다시 말해, 함수 객체의 내부 메서드 `[[Call]]`이 호출되는 것이 아니라 `[[Construct]]`가 호출된다.
  - 단, `new` 연산자와 함께 호출하는 함수는 non-constructor가 아닌 constructor이어야 한다.

```javascript
function add(x, y) {
  return x + y;
}

const instance1 = new add();

console.log(instance1); // add {}
```

```javascript
function createUser(name, role) {
  return { name, role };
}

const instance2 = new createUser('wally', 'FE');

console.log(instance2) ; // {name: 'wally', role: 'FE'}
```

- 반대로 `new` 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출된다.
  - 다시 말해, 함수 객체의 내부 메서드 `[[Construct]]`가 호출되는 것이 아니라 `[[Call]]`이 호출된다.

```javascript
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}

const circle = Circle(5);
console.log(circle); // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter(); // Uncaught TypeError: Cannot read properties of undefined (reading 'getDiameter')
```

- 일반 함수와 생성자 함수에 특별한 형식적 차이는 없다.
  - 따라서 생성자 함수는 일반적으로 파스칼 케이스로 명명하여 일반 함수와 구별할 수 있도록 노력한다.

<br>

### (7) `new.target`

- 생성자 함수가 `new` 연산자 없이 호출되는 것을 방지하기 위해 파스칼 케이스 컨벤션을 사용한다 하더라도 실수는 언제나 발생할 수 있다.
  - 이러한 위험성을 회피하기 위해 ES6에서는 `new.target`을 지원한다.
- `this`와 유사하게 `constructor`인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 <u>메타 프로퍼티</u>라고 부른다.
  - 참고 `new.target`은 IE에서는 지원하지 않는다.
- 함수 내부에서 이를 사용하면 `new` 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다.
  - <b>`new` 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 `new.target`은 함수 자신을 가리킨다.</b>
  - <b>`new` 연산자 없이 일반 함수로서 호출된 함수 내부의 `new.target`은 `undefined`다.</b>

```javascript
function Circle(radius) {
  if (!new.target) {
    return new Circle(radius);
  }
    
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}

const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```

- 만약 IE에서도 이와 비슷한 기능을 제공하고 싶다면 아래와 같은 <b>스코프 세이프 생성자 패턴</b>을 적용할 수 있다.

```javascript
function Circle(radius) {
  if (!(this instanceof Circle)) {
    return new Circle(radius);
  }
    
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}

const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```

- 참고로 대부분의 빌트인 생성자 함수는 `new` 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환한다.
- `Object`, `Function` 생성자 함수
  - `new` 연산자의 존재 여부와 상관 없이 항상 동일하게 동작

```javascript
const obj1 = new Object();
console.log(obj1); // {}

const obj2 = Object();
console.log(obj2); // {}
```

- `String`, `Number`, `Boolean` 생성자 함수
  - `new` 연산자와 함께 호출했을 때 해당 객체를 생성하여 반환
  - `new` 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환

```javascript
const str1 = new String(123);
console.log(str1, typeof str1); // String {'123'} 'object'

const str2 = String(123);
console.log(str2, typeof str2); // 123 string
```

