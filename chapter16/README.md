# Chapter16. 프로퍼티 어트리뷰트

<br>

## 1. 내부 슬롯과 내부 메서드

- ECMAScript 사양에 등장하는 이중 대괄호(`[[...]]`)로 감싼 이름들이 내부 슬롯(internal slot)과 내부 메서드(internal method)다.
- 이 둘은 ECMAScript 사양에 정의된 대로 구현되어 자바스크립트 엔진에서 실제로 동작하지만 개발자가 직접 접근할 수 있도록 외부에 공개된 객체의 프로퍼티는 아니다.
  - 원칙적으로 직접적으로 접근하거나 호출할 수 있는 방법을 제공하지 않지만 일부는 간접적으로 접근할 수 있는 수단을 제공하기는 한다.
  - 예를 들어, `[[Prototype]]` 내부 슬롯의 경우, `__proto__`를 통해 간접적으로 접근할 수 있다.

<br>

## 2. 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

- 자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 <b>프로퍼티 어트리뷰트</b>를 기본값으로 자동 정의한다.
  - 프로퍼티 상태: <u>프로퍼티의 값(value)</u>, <u>값의 갱신 가능 여부(writable)</u>, <u>열거 가능 여부(enumerable)</u>, <u>재정의 가능 여부(configurable)</u>
-  프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯이다.
  - 따라서 직접 접근할 수 없지만 `Object.getOwnPropertyDescriptor` 메서드를 사용하여 간접적으로 확인할 수는 있다.
- `Object.getOwnPropertyDescriptor` 메서드는 프로퍼티 어트리뷰트 정보를 제공하는 <b>프로퍼티 디스크립터 객체</b>를 반환한다.
  - 이 메서드는 하나의 프로퍼티에 대해 프로퍼티 디스크립터 객체를 반환하지만 ES8에서 도입된 `Object.getOwnPropertyDescriptors` 메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.

```javascript
const obj1 = {};
const obj2 = {
  a: 1,
};
const obj3 = {
  x: 100,
  y: 200,
};

// 존재하지 않는 프로퍼티나 상속받은 프로퍼티에 대해서는 undefined가 반환됨
Object.getOwnPropertyDescriptor(obj1, 'a'); // undefined
Object.getOwnPropertyDescriptor(obj2, 'a'); // {value: 1, writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptors(obj3);
// {
//   x: {value: 100, writable: true, enumerable: true, configurable: true},
//   y: {value: 200, writable: true, enumerable: true, configurable: true}
// }
Object.getOwnPropertyDescriptors({}); // {}
```

<br>

## 3. 데이터 프로퍼티와 접근자 프로퍼티

### (1) 데이터 프로퍼티(data property)

- <b>키와 값으로 구성된 일반적인 프로퍼티</b>로 지금까지 살펴본 모든 프로퍼티는 데이터 프로퍼티다.
- 데이터 프로퍼티는 `[[Value]]`, `[[Writable]]`, `[[Enumerable]]`, `[[Configurable]]`와 같은 프로퍼티 어트리뷰트를 갖는다.
  - 이 어트리뷰트들은 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.

| 프로퍼티 어트리뷰트 | 설명                                                         |
| ------------------- | ------------------------------------------------------------ |
| `[[Value]]`         | <ul><li>프로퍼티 키를 통해 <b>프로퍼티 값</b>에 접근하면 반환되는 값이다. </li><li>프로퍼티 키를 통해 프로퍼티 값을 변경하면 `[[Value]]`에 값을 재할당한다. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 `[[Value]]`에 값을 저장한다.</li></ul> |
| `[[Writable]]`      | <ul><li>프로퍼티 값의 <b>변경 가능 여부</b>를 나타내며 <b>boolean 값</b>을 갖는다.</li><li>`[[Writable]]`의 값이 `false`인 경우 해당 프로퍼티의 `[[Value]]`의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다.</li></ul> |
| `[[Enumerable]]`    | <ul><li>프로퍼티의 <b>열거 가능 여부</b>를 나타내며 <b>boolean 값</b>을 갖는다.</li><li>`[[Enumerable]]`의 값이 `false`인 경우 해당 프로퍼티는 `for ... in` 문이나 `Object.keys` 메서드 등으로 열거할 수 없다.</li></ul> |
| `[[Configurable]]`  | <ul><li>프로퍼티의 <b>재정의 가능 여부</b>를 나타내며 <b>boolean 값</b>을 갖는다.</li><li>`[[Configurable]]`의 값이 `false`인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다.</li><li>(단, `[[Writable]]`이 `true`인 경우 `[[Value]]`의 변경과 `[[Writable]]`을 `false`로 변경하는 것은 허용된다.</li></ul> |

<br>

### (2) 접근자 프로퍼티(accessor property)

- 자체적으로는 값을 갖지 않고 <b>다른 데이터 프로퍼티의 값을 읽거나 저장</b>할 때 호출되는 <b>접근자 함수</b>로 구성된 프로퍼티다.

| 프로퍼티 어트리뷰트 | 설명                                                         |
| ------------------- | ------------------------------------------------------------ |
| `[[Get]]`           | <ul><li>접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 <b>읽을 때</b> 호출되는 접근자 함수다. </li><li>즉, 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 `[[Get]]`의 값, 즉 `getter` 함수가 호출되고 그 결과 프로퍼티 값으로 반환된다.</li></ul> |
| `[[Set]]`           | <ul><li>접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 <b>저장할 때</b> 호출되는 접근자 함수다.</li><li>즉, 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 `[[Set]]`의 값, 즉 `setter` 함수가 호출되고 그 결과 프로퍼티 값으로 저장된다.</li></ul> |
| `[[Enumerable]]`    | <ul><li>데이터 프로퍼티의 `[[Enumerable]]`과 같다.</li></ul> |
| `[[Configurable]]`  | <ul><li>데이터 프로퍼티의 `[[Configurable]]`과 같다.</li></ul> |

<br>

### (3) `getter`, `setter` 함수

```javascript
const user = {
  // 데이터 프로퍼티
  name: 'wally',
  age: 28,
    
  // userInfo는 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get userInfo() {
    return `${this.name}(${this.age})`;
  },
    
  // setter 함수
  set userInfo({name, age}) {
    this.name = name;
    this.age = age;
  }
};

console.log(user.name); // 'wally'
console.log(user.age); // 28

user.userInfo = {
  name: 'wally-wally',
  age: 29,
};
console.log(user); // { name: 'wally-wally', age: 29 }

console.log(user.userInfo); // 'wally-wally(29)'

// Object.getOwnPropertyDescriptor 메서드를 통해 반환하는 프로퍼티 디스크립터 객체의 프로퍼티들을 보고 어떤 유형의 프로퍼티인지 구분할 수 있다.
Object.getOwnPropertyDescriptor(user, 'name'); // {value: 'wally-wally', writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptor(user, 'userInfo'); // {enumerable: true, configurable: true, get: ƒ, set: ƒ}
```

- 접근자 프로퍼티 `userInfo`로 프로퍼티 값에 접근시 내부적으로 `[[Get]]` 내부 메서드가 호출될 때 내부에서 일어나는 일

| 단계 | 설명                                                         | 적용                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | 프로퍼티 키가 유효한지 확인하는데 이 때 프로퍼티 키는 문자열 또는 심벌이어야 한다. | 프로퍼티 키 `userInfo`는 문자열이므로 유효한 프로퍼티 키다.  |
| 2    | 프토토타입 체인에서 프로퍼티를 검색한다.                     | `user` 객체에 `userInfo` 프로퍼티가 존재한다.                |
| 3    | 검색된 프로퍼티가 어떤 유형의 프로퍼티인지 확인한다.         | `userInfo` 프로퍼티는 접근자 프로퍼티다.                     |
| 4    | 접근자 프로퍼티의 `[[Get]]`의 값, 즉 `getter` 함수를 호출하여 그 결과를 반영한다. | `userInfo` 프로퍼티의 `[[Get]]`의 값은 `Object.getOwnPropertyDescriptor`  메서드가 반환하는 프로퍼티 디스크립터 객체의 `get` 프로퍼티 값과 같다. |

<br>

## 4. 프로퍼티 정의

- 프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.
- `Object.defineProperty` 메서드를 사용하면 프로퍼티 어트리뷰트를 정의할 수 있다.
  - 인수로는 객체의 참조와 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체를 전달한다.

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
| ----------------------------------- | ---------------------------- | -------------------- |
| `value`                             | `[[Value]]`                  | `undefined`          |
| `get`                               | `[[Get]]`                    | `undefined`          |
| `set`                               | `[[Set]]`                    | `undefined`          |
| `writable`                          | `[[Writable]]`               | `false`              |
| `enumerable`                        | `[[Enumerable]]`             | `false`              |
| `configurable`                      | `[[Configurable]]`           | `false`              |

```javascript
const obj = {};

// 데이터 프로퍼티 정의
Object.defineProperty(obj, 'id', {
  value: 1,
  writable: true,
  enumerable: false,
  configurable: true
});
Object.defineProperty(obj, 'name', {
  value: 'Macbook',
  writable: false,
  enumerable: true,
  configurable: false
});

// writable이 false인 경우 해당 프로퍼티의 value의 값을 변경할 수 없다.
// 이때 값을 변경하면 에러는 발생하지 않고 무시된다.
console.log(obj); // {name: "Macbook", id: 1}
obj.name = 'Macbook2';
console.log(obj); // {name: "Macbook", id: 1}

// enumerable이 false인 경우 해당 프로퍼티는 for ... in 문이나 Object.keys 등으로 열거할 수 없다.
for (const p in obj) {
  console.log(p); // 'name'
}

// configurable이 false인 경우 해당 프로퍼티를 삭제할 수 없다.
// 이때 프로퍼티를 삭제하면 에러는 발생하지 않고 무시된다.
delete obj.name;
console.log(obj.name); // 'Macbook'
// 그리고 configurable이 false인 경우 해당 프로퍼티를 재정의할 수 없다.
Object.defineProperty(obj, 'name', {writable: true}); // Uncaught TypeError: Cannot redefine property: name

// 접근자 프로퍼티 정의
Object.defineProperty(obj, 'c', {
  get() {
    return `${this.a}-${this.b}`;
  },
  set({a, b}) {
    this.a = a;
    this.b = b;
  },
  enumerable: true,
  configurable: true,
});
```

- `Object.defineProperty` 메서드는 한 번에 하나의 프로퍼티만 정의할 수 있다.
- `Object.defineProperties` 메서드를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있다.

```javascript
const newObj = Object.defineProperties({}, {
  id: {
    value: 1,
    writable: true,
    enumerable: false,
    configurable: true
  },
  name: {
    value: 'Macbook',
    writable: false,
    enumerable: true,
    configurable: false
  }
});

console.log(newObj); // {name: "Macbook", id: 1}
```

<br>

## 5. 객체 변경 방지

### (1) 객체 확장 금지

- <b>`Object.preventExtensions` 메서드</b>는 객체의 <b>확장을 금지</b>한다.
  - 기존 프로퍼티는 그대로 두고 추가하는 동작만 할 수 없도록 막는 기능을 한다.(즉, 할당, 삭제, 속성 변경은 모두 가능)
  - 프로퍼티 동적 추가와 `Object.defineProperty` 메서드로 추가하는 두 방법 모두 금지된다.
- 확장 금지 여부는 <b>`Object.isExtensible` 메서드</b>로 알 수 있다.

```javascript
var myObj = {
  a: 2,
};

Object.preventExtensions(myObj);
myObj.b = 3; // (기본적으로 무시되나 strict mode에서는 에러)
console.log(myObj.b); // 에러는 발생하지 않으나 해당 프로퍼티가 없으므로 undefined가 출력된다.(단, strict mode에서는 TypeError)
```

<br>

### (2) 객체 밀봉

- <b>`Object.seal` 메서드</b>는 객체를 <b>밀봉(봉인)</b>한다.
  - 즉, 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의가 금지된다.
  - 밀봉된 객체는 읽기와 쓰기만 가능하다.
- `Object.preventExtensions` 메서드를 적용하고 데이터 프로퍼티를 모두 `configurable: false`로 처리하는 액션과 동일한 효과를 가진다.
- 밀봉 여부는 <b>`Object.isSealed` 메서드</b>로 알 수 있다.

```javascript
var myObj = {};
Object.defineProperty(myObj, 'num', {
  value: 10,
  writable: true,
  enumerable: true,
  configurable: true,
});

Object.seal(myObj);

// 프로퍼티 읽기 가능
console.log(myObj.num); // 10

// 프로퍼티 값 변경 가능
myObj.num = 20;
console.log(myObj.num); // 20

// 새로운 프로퍼티 추가 불가능(기본적으로 무시되나 strict mode에서는 에러)
myObj.num2 = 30;
console.log(myObj); // {num: 20}

// 프로퍼티 속성 변경 불가능(Object.seal()로 봉인하지 않았으면 가능)
Object.defineProperty(myObj, 'num', { enumerable: false }); // Uncaught TypeError: Cannot redefine property: num

// 프로퍼티 삭제 불가능(기본적으로 무시되나 strict mode에서는 에러)
delete myObj.num;
console.log(myObj); // {num: 20}
```

<br>

### (3) 객체 동결

- <b>`Object.freeze` 메서드</b>를 이용해서 프로퍼티 <b>읽기만 가능</b>한 동결 객체를 만들 수 있다.
- `Object.seal` 메서드를 적용하고 데이터 프로퍼티를 모두 `writable: false`로 처리하는 액션과 동일한 효과를 가진다.
- 동결 여부는 <b>`Object.isFrozen` 메서드</b>로 알 수 있다.

```javascript
var obj = {
  a: {
    x: 2,
  },
  b: {
    y : 3
  },
  c: 10,
};

Object.freeze(obj);

// 프로퍼티 추가 금지(기본적으로 무시되나 strict mode에서는 에러)
obj.d = 20;
console.log(obj); // { a: {x: 2}, b: {y: 3}, c: 10 }

// 프로퍼티 삭제 금지(기본적으로 무시되나 strict mode에서는 에러)
delete obj.c;
console.log(obj); // { a: {x: 2}, b: {y: 3}, c: 10 }

// 프로퍼티 값 갱신 금지(기본적으로 무시되나 strict mode에서는 에러)
obj.c = 20;
console.log(obj); // { a: {x: 2}, b: {y: 3}, c: 10 }

// 프로퍼티 어트리뷰트 재정의 금지
Object.defineProperty(obj, 'c', { configurable: true }); // Uncaught TypeError: Cannot redefine property: c

console.log(obj.a); // {x: 2}

obj.a = 5;
console.log(obj); // { a: {x: 2}, b: {y: 3}, c: 10 }

// [주의!] 얕은 불변성만 지원하므로 그 안의 다른 참조 타입의 값이 있다면 재귀적으로 반복하면서 객체를 완전히 동결해야 한다.
obj.a.x = 3;
console.log(obj); // { a: {x: 3}, b: {y: 3}, c: 10 }
```

- 이 메서드와 `const` 변수 선언 키워드를 사용하여 주로 값이 변하지 않는 상수를 관리하는 객체를 만들 수 있다.

```javascript
/** 메일 쓰기 페이지 모드 상수 값 */
const MAIL_WRITE_MODE = {
  /** 신규 작성 */
  new: 'new',
  /** 답장 */
  reply: 'reply',
  /** 전체 답장 */
  reply_all: 'reply_all',
  /** 전달 */
  forward: 'forward',
  /** 임시 보관함에서 가져온 메일 */
  temp: 'temp',
  /** 재발송 */
  resend: 'resend',
};

Object.freeze(MAIL_WRITE_MODE);

export {
  MAIL_WRITE_MODE,
}
```

<br>

### (4) 불변 객체

- 위 예제에서 마지막 부분에서 봤다시피 지금까지 살펴본 객체 변경 방지 메서드들은 <b>얕은 불변성</b>만 지원한다.
- 따라서 `Object.freeze` 메서드로 객체를 동결하여도 중첩 객체까지 동결할 수 없다.
- 이를 해결하기 위해 중첩 객체의 모든 프로퍼티에 <b>재귀적으로 `Object.freeze` 메서드를 호출</b>해야 한다.

```javascript
function deepFreeze(target) {
  // 객체가 아니거나 동결된 객체는 무시하고 객체이고 동결되지 않은 객체만 동결한다.
  if (target && typeof target === 'object' && !Object.isFrozen(target)) {
    Object.freeze(target);
    // 모든 프로퍼티를 순회하며 재귀적으로 동결한다.
    Object.keys(target).forEach(key => deepFreeze(target[key]));
  }
  return target;
}

const obj = {
  a: {
    x: 2,
  },
  b: {
    y : 3
  },
};

deepFreeze(obj);

console.log(Object.isFrozen(obj)); // true
console.log(Object.isFrozen(obj.a)); // true

obj.a.x = 4;
console.log(obj); // { a: {x: 2}, b: {y: 3} }
```

<br>

### :pushpin: 객체 변경 방지 메서드 정리

| 동작                     | 일반 객체 | 동결 객체(freeze) | 봉인 객체(seal) | 확장 금지 객체(preventExtensions) |
| ------------------------ | --------- | ----------------- | --------------- | --------------------------------- |
| 프로퍼티 추가            | O         | X                 | X               | X                                 |
| 프로퍼티 값 읽기         | O         | O                 | O               | O                                 |
| 프로퍼티 값 설정(쓰기)   | O         | X                 | O               | O                                 |
| 프로퍼티 어트리뷰트 변경 | O         | X                 | X               | O                                 |
| 프로퍼티 삭제            | O         | X                 | X               | O                                 |
