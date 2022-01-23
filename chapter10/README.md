# Chapter10. 객체 리터럴

<br>

## 1. 객체(object)란?

- 원시 값과 객체 타입 비교
  - 원시 타입은 단 하나의 값만 나타내지만 객체 타입은 다양한 타입의 값(원시 값 또는 다른 객체)을 하나의 단위로 구성한 복합적인 자료구조이다.
  - 원시 타입의 값, 즉 <b>원시 값은 변경 불가능한 값</b>(immutable value)이지만 객체 타입의 값, 즉 <b>객체는 변경 가능한 값</b>(mutable value)이다.
- 객체의 구성
  - 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 <u>키(key)와 값(value)</u>으로 구성된다.
  - 자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있다.
  - 자바스크립트의 함수는 <u>일급 객체</u>이므로 값으로 취급할 수 있어 함수도 프로퍼티 값으로 사용할 수 있다.
    - 이 때 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메서드라 부른다.
  - 프로퍼티는 객체의 상태를 나타내는 값(data)을, 메서드는 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)을 의미한다.

---

### :heavy_plus_sign: 평가와 일급

> `Chapter06` 에서 추가 내용으로 살펴본 내용이지만, '일급 객체'라는 용어가 등장했으므로 복습 차원에서 다시 한 번 살펴보자.

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

- 예시 코드로 객체 구조 간략히 살펴보기

  ```javascript
  const counter = {
    num: 0,
    increase: function() {
      this.num += 1;
    }
  }
  ```

  - `num: 0`, `increase: function() { this.num += 1; }` 모두 프로퍼티에 속하며 `increase` 부분은 메서드라고 부른다.
  - `num`, `increase`는 프로퍼티의 key에 해당하고, `0`, `function() { this.num += 1; }` 부분은 프로퍼티의 value에 해당한다.

-  객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 <u>객체지향 프로그래밍</u>이라 한다.

<br>

## 2. 객체 리터럴에 의한 객체 생성

- 자바스크립트에서 객체 생성하는 방법
  - 객체 리터럴
  - `Object ` 생성자 함수
  - 생성자 함수
  - `Object.create` 메서드
  - 클래스(ES6)
- 객체 리터럴에 의한 객체 생성
  - 객체 리터럴은 중괄호(`{ ... }`) 내에 0개 이상의 프로퍼티를 정의한다.
  - 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성한다.
  - 만약 중괄호 내에 프로퍼티를 정의하지 않으면 빈 객체가 생성된다.

```javascript
const user = {
  name: 'wally',
  age: 29,
  sayHi: function() {
    console.log(`Hello! My name is ${this.name}!`);
  }
};

console.log(typeof user); // object
console.log(user); // { name: 'wally', age: 29, sayHi: ƒ }
```

```javascript
// 빈 객체
const empty = {};

console.log(typeof empty); // object
console.log(empty); // {}
```

- 코드 블록의 닫는 중괄호 뒤에는 세미 콜론을 붙이지 않았지만 객체 리터럴은 값으로 평가되는 표현식이므로 객체 리터럴의 닫는 중괄호 뒤에는 세미콜론을 붙인다.
- 객체 리터럴 방식은 클래스와 달리 숫자 값이나 문자열을 만드는 것과 유사하게 리터럴로 객체를 생성한다.
  - 객체 리터럴에 프로퍼티를 포함시켜 객체를 생성함과 동시에 프로퍼티를 만들 수 있고, 객체를 생성한 이후에 프로퍼티를 동적으로 추가할 수도 있다.
  - 이것이 바로 자바스크립트의 유연함과 강력함을 대표하는 객체 생성 방식인 '객체 리터럴'이다.

<br>

## 3. 프로퍼티(property)

### (1) 프로퍼티의 특징

```javascript
const person = {
  name: 'Lee',
  age: 30,
};
```

- <b>객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.</b>
- 프로퍼티를 나열할 때는 쉼표(`,`)로 구분하고 일반적으로 마지막 프로퍼티 뒤에는 쉼표를 사용하지 않으나 사용해도 좋다.
- 프로퍼티의 키와 값

| 구분      | 설명                                                         |
| --------- | ------------------------------------------------------------ |
| 키(key)   | 빈 문자열을 포함하는 모든 문자열 또는 심벌 값(빈 문자열도 가능하지만 키로서의 의미를 갖지 못하므로 권장하지 않음) |
| 값(value) | 자바스크립트에서 사용할 수 있는 모든 값                      |

<br>

### (2) 프로퍼티 키

- 프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로서 <b>식별자 역할</b>을 한다.

- 심벌 값도 프로퍼티 키로 사용할 수 있지만 일반적으로 문자열을 사용한다.

  - 이때 프로퍼티 키는 문자열이므로 따옴표로 묶어야 한다.
  - 하지만 식별자 네이밍 규칙을 준수하는 이름, 즉 자바스크립트에서 사용 가능한 유효한 이름인 경우 따옴표를 생략할 수 있다.
  - 반대로 말하면 식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용해야 한다.

- 식별자 네이밍 규칙을 따르지 않는 프로퍼티 키를 사용한 경우

  ```javascript
  const person = {
    firstName: 'Ung-mo', // 식별자 네이밍 규칙 준수함
    'last-name': 'Lee', // 식별자 네이밍 규칙 준수하지 않음
  };
  
  console.log(person); // {firstName: 'Ung-mo', last-name: 'Lee'}
  ```

  - 위 코드에서 `firstName` 프로퍼티 키는 식별자 네이밍 규칙을 준수하므로 따옴표를 작성하지 않아도 되지만, `last-name`은 준수하지 않았기 때문에 반드시 따옴표를 작성해야 한다.
  - 자바스크립트 엔진은 따옴표를 생략한 `last-name`을 `-` 연산자가 있는 표현식으로 해석한다.

  ```javascript
  const person = {
    firstName: 'Ung-mo',
    last-name: 'Lee',
  };
  
  // Uncaught SyntaxError: Unexpected token '-'
  ```

- 문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 키를 <u>동적으로 생성</u>할 수 있다.

  - 이 경우에는 프로퍼티 키로 사용할 표현식을 대괄호로 묶어야 한다.

  ```javascript
  let obj = {};
  const key = 'hello';
  
  // ES5: 프로퍼티 키 동적 생성
  obj[key] = 'world';
  
  // ES6: computed property name
  // obj = { [key]: 'world' };
  
  console.log(obj); // {hello: 'world'}
  ```

- 프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 <u>암묵적 타입 변환을 통해 문자열이 된다.</u>

  - 예를 들어, 프로퍼티 키로 숫자 리터럴을 사용하면 따옴표는 붙지 않지만 내부적으로는 문자열로 변환된다.

  ```javascript
  const obj1 = {
    0: 123,
    1: 456,
  };
  
  console.log(obj1); // {0: 123, 1: 456}
  
  console.log(obj1[0]); // 123
  console.log(obj1['0']); // 456
  ```

  ```javascript
  const obj2 = {
    true: 123,
    false: 456,
  };
  
  console.log(obj2); // {true: 123, false: 456}
  
  console.log(obj2[true]); // 123
  console.log(obj2['true']); // 123
  
  console.log(obj2[false]); // 456
  console.log(obj2['false']); // 456
  
  console.log(obj2[!!false]); // ?
  console.log(obj2[!!'false']); // ?
  ```

- `var`, `function`과 같은 예약어를 프로퍼티 키로 사용해도 에러가 발생하지 않으나 예상치 못한 에러가 발생할 여지가 있으므로 권장하지 않는다.

- 또한 이미 존재하는 프로퍼티 키를 중복 선언하면 <u>나중에 선언한 프로퍼티 키가 먼저 선언한 프로퍼티를 덮어쓴다.</u>

  - 이때 에러가 발생하지 않는다는 점에 주의하자.

  ```javascript
  const obj1 = {
    name: 'mally',
    name: 'wally',
  };
  
  console.log(obj1.name); // 'wally'
  ```

<br>

## 4. 메서드(method)

- 자바스크립틔 함수는 일급 객체다.
  - 함수는 값으로 취급할 수 있기 때문에 프로퍼티 값으로 사용할 수 있다.
- 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 <b>메서드(method)</b>라 부른다.
  - 즉, 메서드는 객체에 묶여 있는 함수를 의미한다.

```javascript
const circle = {
  radius: 5,
  getDiameter: function() {
    return 2 * this.radius; // this는 circle 객체를 가리킨다.
  }
};

console.log(circle.getDiameter()); // 10
```

- 메서드 내부에서 사용한 `this` 키워드는 객체 자신을 가리키는 참조 변수가 된다.

  - 이에 대해서는 나중에 더 자세히 살펴보자.

  ```javascript
  const obj = {
    v1: 12,
    v2: 34,
    f1: function() {
      console.log(this);
    },
    f2: () => {
      console.log(this);
    },
    f3: function() {
      function subf3() {
        console.log(this);
      }
        
      subf3();
    },
    f4: function() {
      const subf4 = () => {
        console.log(this);
      }
        
      subf4();
    },
  }
  
  console.log(obj.f1()); // ?
  console.log(obj.f2()); // ?
  console.log(obj.f3()); // ?
  console.log(obj.f4()); // ?

<br>

## 5. 프로퍼티 접근

- 프로퍼티에 접근하는 방법
  - 마침표 프로퍼티 접근 연산자(`.`)를 사용하는 <b>마침표 표기법</b>
  - 대괄호 프로퍼티 접근 연산자(`[ ... ]`)를 사용하는 <b>대괄호 표기법</b>

```javascript
const person = {
  name: 'Lee',
};

console.log(person.name); // Lee
console.log(person['name']); // Lee
```

- 대괄호 표기법을 사용하는 경우 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다.
- 또한 객체에 존재하지 않는 프로퍼티에 접근하면 `undefined`를 반환한다.
  - 이때 `ReferenceError`가 발생하지 않는데 주의하자.

```javascript
const person = {
  name: 'Lee',
};

console.log(person.age); // undefined
```

- 프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않는 이름, 즉 자바스크립트에서 사용 가능한 유효한 이름이 아니면 반드시 대괄호 표기법을 사용해야 한다.
  - 단, 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있따.
  - 그 외의 경우 대괄호 내에 들어가는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다는 점을 잊지 말자.

```javascript
const person = {
  'last-name': 'Lee',
  1: 123,
};

person.'last-name'; // Uncaught SyntaxError: Unexpected string
person.last-name; // 브라우저 환경: NaN / Node.js 환경: ReferenceError: name is not defined

person[last-name]; // Uncaught ReferenceError: last is not defined
person['last-name']; // Lee

person.1; // Uncaught SyntaxError: Unexpected number
person.'1'; // Uncaught SyntaxError: Unexpected string
person[1]; // 123 : person[1] => person['1']
person['1']; // 123
```

- 위 코드에서 `person.last-name`이 구동하는 환경에 따라 결과가 다르게 나오는 이유를 간략히 살펴보자.
  - `person.last-name`을 실행할 때 자바스크립트 엔진은 `person.last`를 먼저 평가하는데 평가 결과는 `undefined`가 된다.
  - `person.last-name` 은 `undefined-name`과 같고 자바스크립트 엔진은 이어서 `name` 식별자를 찾는데 이때 `name`을 프로퍼티 키가 아닌 <u>식별자로 해석</u>되는 것에 주의해야 한다.
  - Node.js 환경에서는 어디에도 `name` 식별자 선언이 없으므로 `ReferenceError`가 발생하지만 브라우저 환경에서는 `name` 이라는 전역 변수가 암묵적으로 존재한다.(`window.name`은 현재 열린 창을 의미하며 기본값은 빈 문자열임)
  - 그래서 브라우저 환경에서 `person.last-name`은 `undefined - ''`와 같으므로 `NaN`이 된다.
- 그렇다면 아래와 같은 코드에서는 어떤 결과가 반환될지 예상해보자.

```javascript
const obj = {
  'hello-world': 'wow',
};

console.log(obj.hello-world); // ?
```

<br>

## 6. 프로퍼티의 C, U, D

### (1) 프로퍼티 값 갱신(Update)

```javascript
const person = {
  name: 'Lee',
};

person.name = 'Kim';

console.log(person); // {name: 'Kim'}
```

<br>

### (2) 프로퍼티 동적 생성(Create)

```javascript
const person = {
  name: 'Lee',
};

person.age = 30;

console.log(person); // {name: 'Lee', age: 30}
```

<br>

### (3) 프로퍼티 삭제(Delete)

```javascript
const person = {
  name: 'Lee',
  age: 30,
};

// delete 연산자로 객체의 프로퍼티를 삭제한다.
// 이때 delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 한다.
delete person.age;

// 만약 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 무시된다.
delete person.address;

console.log(person); // {name: 'Lee'}
```

<br>

## 7. ES6에서 추가된 객체 리터럴의 확장 기능

### (1) 프로퍼티 축약 표현(property shorthand)

```javascript
// ES5
var x = 1;
var y = 2;

var obj = {
  x: x,
  y: y,
};

console.log(obj); // {x: 1, y: 2}
```

```javascript
// ES6
// 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있고 이때 프로퍼티 키는 변수 이름으로 자동 생성된다.
const x = 1;
const y = 2;

const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

<br>

### (2) 계산된 프로퍼티 이름(computed property name)

- 문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다.
  - 단, 프로퍼티 키로 사용할 표현식을 대괄호로 묶어야 하는데 이를 <b>계산된 프로퍼티 이름(computed property name)</b>이라 한다.

```javascript
// ES5
// 객체 리터럴 외부에서 대괄호 표기법을 사용해서 프로퍼티 키를 동적 생성함

var prefix = 'prop';
var index = 0;

var obj = {};

obj[prefix + '-' + ++index] = index;
obj[prefix + '-' + ++index] = index;
obj[prefix + '-' + ++index] = index;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

```javascript
// ES6
// 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성할 수 있음

const prefix = 'prop';
let index = 0;

const obj = {
  [`${prefix}-${++index}`]: index,
  [`${prefix}-${++index}`]: index,
  [`${prefix}-${++index}`]: index,
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

```javascript
// 다른 방식으로 코드 개선

const prefix = 'prop';
const indexArr = Array(3)
  .fill(undefined)
  .map((_, index) => index + 1);

const obj = indexArr.reduce((accObj, currentIndex) => {
  return {
    ...accObj,
    [`${prefix}-${currentIndex}`]: currentIndex,
  };
}, {});

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

---

### :heavy_plus_sign: Vue.js 프로젝트에서 computed property name을 적용한 사례

> 아래 코드는 Vue.js로 작성한 코드 중 vuex의 `getters` 코드 일부를 발췌한 것이다. (typescript 언어로 작성됨에 유의)

```typescript
import { FileboxState } from './state';

export enum FileboxGettersType {
  GET_FILEBOX_LIST = 'Filebox/GET_FILEBOX_LIST',
  GET_FILEBOX_TOTAL_SIZE = 'Filebox/GET_FILEBOX_TOTAL_SIZE',
}

export const fileboxGetters = {
  [FileboxGettersType.GET_FILEBOX_LIST](state: FileboxState) {
    return state.fileboxList;
  },

  [FileboxGettersType.GET_FILEBOX_TOTAL_SIZE](state: FileboxState) {
    return state.fileboxList.reduce((sum, { file_size }) => sum + file_size, 0);
  },
};

export type FileboxGetters = typeof fileboxGetters;
```

#### :round_pushpin: 코드를 이해하는데 도움이 될 만한 개념들

- `enum`

  - 타입스크립트에 있는 문법 중 하나로 특정 값들의 집합을 의미하는 자료형을 의미한다.
  - 숫자형 이넘, 문자형 이넘이 있다.

  ```typescript
  // 숫자형 이넘
  enum Direction {
    Up = 0,
    Down = 1,
    Left = 2,
    Right = 3,
  }
  
  Direction.Up // 0
  ```

  ```typescript
  // 문자형 이넘
  enum Direction {
    Up = 'U',
    Down = 'D',
    Left = 'L',
    Right = 'R',
  }
  
  Direction.Up // 'U'
  ```

- vuex의 `getters`

  - vuex에는 상태를 관리하는 `state`가 있는데 만약 아래 코드 처럼 `state`를 이용하여 두 군데의 vue 파일에서 중복된 코드가 있다고 가정해보자.

  ```vue
  <!-- A.vue -->
  
  <script>
  export default {
    computed: {
      uploadFileCount() {
        return this.$store.state.uploadFile.length;
      },
    }
  }
  </script>
  ```

  ```vue
  <!-- B.vue -->
  
  <script>
  export default {
    computed: {
      uploadFileCount() {
        return this.$store.state.uploadFile.length;
      },
    }
  }
  </script>
  ```

  - 여러 컴포넌트에서 같은 로직을 비효율적으로 중복 사용하고 있는데 vuex의 `state`의 변경이 일어날 때마다 수행되는 로직을 각 컴포넌트의 `computed`에 작성하지 않고 vuex에서 수행되도록 할 수 있는데 이를 `getters` 속성이라고 한다.
  - `getters`를 사옹하면 코드 가독성도 올라가고 성능에서도 이점이 생긴다.

  ```javascript
  // store.js (vuex)
  getters: {
    uploadFileCount(state) {
      return state.uploadFile.length;
    }
  }
  ```

  ```vue
  <!-- A.vue -->
  
  <script>
  export default {
    computed: {
      uploadFileCount() {
        return this.$store.getters.uploadFileCount;
      },
    }
  }
  </script>
  ```

  ```vue
  <!-- B.vue -->
  
  <script>
  export default {
    computed: {
      uploadFileCount() {
        return this.$store.getters.uploadFileCount;
      },
    }
  }
  </script>
  ```

#### :round_pushpin: vuex의 `getters` 에 computed property name 적용

![getters](https://user-images.githubusercontent.com/52685250/150671642-85653b22-23e3-47ed-9f0d-35059bebb016.png)

- `fileboxGetters` 라는 객체 안에 선언된 프로퍼티 키 값을 별도로 관리하기 위해 `enum` 이라는 열거형 자료형을 사용했고 `enum`에서 선언한 각 `getter`의 이름을 가져와서 `fileboxGetters` 의 프로퍼티 키 값으로 사용하고 있다.
- 대괄호 표기법을 통해 작성된 `fileboxGetters` 객체는 실제로 위 이미지와 같이 해석되고, `fileboxGetters['Filebox/GET_FILEBOX_LIST']`와 같이 접근할 수 있다.
- 참고로 실제로 `vue` 파일에서는 `fileboxGetters['Filebox/GET_FILEBOX_LIST']`와 같이 불러와서 값을 사용하지 않고 `enum` 타입을 export 하여 사용할 `getters` 의 이름을 여기서 가져와서 사용하고 있다.
- 더 자세한 내용은 `vuex`, `typescript` 개념을 우선 이해하고 다시 살펴보는 것을 추천한다.

```vue
<script lang="ts">
import Vue from 'vue';
import { CheckedFilebox } from './types';
import { FileboxGettersType } from '@/store/filebox/getters';
    
export default Vue.extend({
  computed: {
    fileboxList(): CheckedFilebox[] {
      return this.$store.getters[FileboxGettersType.GET_FILEBOX_LIST];
    },
  }
});
</script>
```

---

<br>

### (3) 메서드 축약 표현

```javascript
// ES5
var obj = {
  name: 'Lee',
  sayHi: function() {
    console.log('Hi! ' + this.name);
  },
};

obj.sayHi(); // Hi! Lee
```

```javascript
// ES6
const obj = {
  name: 'Lee',
  sayHi() {
    console.log(`Hi! ${this.name}`);
  },
};

obj.sayHi!(); // Hi! Lee
```
