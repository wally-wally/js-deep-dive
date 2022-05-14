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

(작성중...)
