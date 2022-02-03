# Chapter11. 원시 값과 객체의 비교

<br>

## :hand: Intro - 원시 타입과 객체(참조) 타입의 큰 차이점

- 원시 타입의 값(원시값)은 <b>변경 불가능한 값(immutable value)</b>이고 객체 타입의 값(객체)은 <b>변경 가능한 값(mutable value)</b>이다.
- 원시 값을 변수에 할당하면 변수에는 <b>실제 값이 저장</b>되고 객체를 변수에 할당하면 변수에는 <b>참조 값이 저장</b>된다.
- 원시 값을 갖는 변수를 다른 변수에 할당하면 원본의 <b>원시 값이 복사되어 전달(값에 의한 전달; pass by value)</b>되고 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 <b>참조 값이 복사되어 전달(참조에 의한 전달; pass by reference)</b>된다.

<br>

## 1. 원시 값

### (1) 변경 불가능한 값

- 원시 타입의 값, 즉 원시값은 <b>변경 불가능한 값(immutable value)</b>이다.
  - 다시 말해, 한번 생성된 원시 값은 <b>읽기 전용</b> 값으로서 변경할 수 없다는 의미이다.
  - 이와 같은 원시 값의 특성에 의해 <u>데이터의 신뢰성</u>을 보장한다.
- 이 때 변수가 아닌 값이 변경 불가능하다는 것이 포인트이다.
  - 원시 값 자체를 변경할 수는 없지만 변수는 언제든지 재할당을 통해 변수 값을 변경할 수 있다.
- 참고로 변수의 상대 개념인 상수는 재할당이 금지된 변수를 말한다.
  - 상수도 값을 저장하기 위한 메모리 공간이 필요하므로 변수라고 할 수 있지만 상수는 변수에 비해 단 한 번만 할당이 허용되므로 변수 값을 변경할 수 없다.

```javascript
// const 키워드를 사용해 선언한 변수는 재할당이 금지됨
const obj = {};

// 하지만 const 키워드를 사용해 선언한 변수에 할당한 객체는 변경할 수 있음
obj.a = 1;
console.log(obj); // {a: 1}
```

- 'Chapter04. 변수'에서 이전에 살펴보았듯이 값을 재할당할 때 이전의 원시 값을 변경하는 것이 아니라 <u>새로운 메모리 공간을 확보</u>하고 <u>재할당한 원시 값을 저장</u>한 후, 변수는 <u>새롭게 재할당한 원시 값을 가리킨다.</u>
  - 이 때 변수가 참조하던 메모리 공간의 주소가 바뀌는데 이는 변수에 할당된 원시 값이 변경 불가능한 값이기 때문이다.

![01](https://user-images.githubusercontent.com/52685250/145706348-cdb3b482-e910-4d76-b4a6-5ff47420945c.JPG)

- 불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없다.
  - 만약 다른 방법이 존재한다면 예기치 않게 변수 값이 변경될 수 있다는 것을 의미하며 값의 변경, 즉 상태 변경을 추적하기 어렵게 만든다.

<br>

### (2) 문자열과 불변성

- 다른 언어와 다른 자바스크립의 문자열 특성
  - C에서는 문자열을 문자의 배열로 처리하고 자바에서는 문자열을 `String` 객체로 처리한다.
  - 하지만 자바스크립트에서는 개발자의 편의를 위해 <b>원시 타입인 문자열 타입을 제공</b>한다.
- 문자열의 일부 문자 변경해보기
  - 문자열은 <u>유사 배열 객체(array-like object)</u>이면서 <u>이터러블(iterable)</u>이므로 배열과 유사하게 각 문자에 접근할 수 있다.
  - 이 때 이미 생성된 문자열의 일부 문자를 변경해도 반영되지 않는데 이는 <b>문자열은 변경 불가능한 값(읽기 전용)</b>이기 때문이다.

```javascript
var str = 'abcde';

// 문자열은 유사 배열이므로 배열과 유사하게 인덱스를 사용해 각 문자에 접근할 수 있다.
console.log(str[0]); // 'a'

// 원시 값인 문자열이 객체처럼 동작한다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // 'ABCDE'

// 하지만 문자열은 원시 값이므로 변경할 수 없다. 이때 에러가 발생하지 않는다는 것에 유의하자.
str[0] = 'h';

console.log(str); // 'abcde'
```

---

### :heavy_plus_sign: 유사 배열 객체(array-like object)

- 말 그대로 배열이 아닌데 배열인 척 하는 것을 유사 배열 객체라고 부른다.
- 유사 배열 객체가 되기 위한 조건
  - 반드시 <b>`length` 프로퍼티</b>를 갖고 있어야 한다.
  - 가급적이면 index 번호가 0번부터 시작해서 1씩 증가해야 한다. 만약 이를 지키지 않게되면 예상치 못한 결과가 생긴다.

```javascript
// 유사 배열 객체 예시
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

// 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있다.
for (let index = 0; index < arrayLike.length; index += 1) {
  console.log(arrayLike[index]); // 1 2 3
}
```

```javascript
// index 번호가 0번부터 시작해서 1씩 증가하지 않은 경우
const arrayLike = {
  0: 1,
  2: 2,
  3: 3,
  length: 3,
};

for (let index = 0; index < arrayLike.length; index += 1) {
  console.log(arrayLike[index]); // 1 undefined 3
}

console.log(arrayLike); // {0: 1, 2: 2, 3: 3, length: 3}
console.log(Array.from(arrayLike)); // [1, undefined, 2]
```

- 일반적으로 유사 배열 객체는 이터러블이 아닌 일반 객체이므로 `Symbol.iterator` 메서드가 없어 `for ... of` 문으로 순회할 수 없다.

```javascript
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

for (const item of arrayLike) {
  console.log(item); // Uncaught TypeError: arrayLike is not iterable
}
```

- 단, `arguments`, `NodeList`, `HTMLCollection` 은 유사 배열 객체이면서 이터러블이다.
  - 왜냐하면 이 객체에 `Symbol.iterator` 메서드를 구현되어 있기 때문이다.
  - 또한 이터러블이 된 이후에도 `length` 프로퍼티를 가지며 인덱스로 접근할 수 있는 것에는 변함이 없어 이들은 유사 배열 객체이면서 이터러블인 것이다.
- 모든 유사 배열 객체가 이터러블인 것은 아니기 때문에 이를 해결하기 위해 ES6에서 `Array.from()` 메서드가 등장했다.
  - 이는 유사 배열 객체를 진짜 배열로 변환해주는데 자세한 내용은 [MDN 공식 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from)를 통해 살펴보자.

```javascript
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

const arr = Array.from(arrayLike);
console.log(arr); // [1, 2, 3]

for (const item of arr) {
  console.log(item); // 1 2 3
}
```

---

<br>

### (3) 값에 의한 전달

```javascript
var score = 80;

var copy = score;

// 변수에 원시 값을 갖는 변수를 할당하면 할당받는 변수(copy)에는 할당되는 변수(score)의 원시 값이 복사되어 전달되는데 이를 '값에 의한 전달'이라 한다.
// 하지만 score, copy 변수는 숫자 값 80을 갖지만 두 변수의 값 80은 서로 다른 메모리 공간에 저장된 별개의 값이다.
console.log(score, copy); // 80 80
console.log(score === copy); // true

// 두 변수의 값은 다른 메모리 공간에 저장된 별개의 값이므로 score 변수의 값을 변경해도 copy 변수의 값에는 어떠한 영향도 없다.
score = 100;

console.log(score, copy); // 100 80
console.log(score === copy); // false
```

- '값에 의한 전달'을 엄격하게 표현하면 변수에는 값이 전달되는 것이 아니라 <b>메모리 주소가 전달</b>되는데 변수와 같은 식별자는 값이 아니라 메모리 주소를 기억하고 있기 때문이다.
  - 단, 전달된 메모리 주소를 통해 메모리 공간에 접근하면 값을 참조할 수 있다.
  - 변수를 할당하거나 두 변수 중 어느 하나의 변수에 값을 재할당할 때 두 변수의 원시 값은 <b>서로 다른 메모리 공간에 저장된 별개의 값이 되어 어느 한쪽에서 재할당을 통해 값을 변경하더라도 서로 간섭할 수 없다.</b>
- 'Chapter04. 변수'에서 살펴보았던 '식별자' 내용을 다시 살펴보자.
  - 식별자는 어떤 값을 구별해서 식별할 수 있는 고유한 이름이라고 했다.
  - 값은 메모리 공간에 저장되어 있어 메모리 공간에 저장되어 있는 어떤 값을 구별해서 식별해낼 수 있어야 하므로 값 자체를 기억하는 것이 아니라 해당 값에 저장되어 있는 <b>메모리 주소를 기억</b>한다.

<br>

## 2. 객체

(작성중...)
