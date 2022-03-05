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
  - 그리고 객체에 선언한 키-값 쌍의 개수 만큼 `length` 프로퍼티의 값으로 지정해주는 것이 좋다.

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

```javascript
// length 프로퍼티가 객체에 선언한 키-값 쌍의 개수보다 적은 경우
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 1,
};

console.log(Array.from(arrayLike)); // [1]
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
  - 이는 유사 배열 객체를 진짜 배열로 변환(`Symbol.iterator` 메서드도 있음해주는데 자세한 내용은 [MDN 공식 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from)를 통해 살펴보자.

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

### (1) 자바스크립트 객체의 관리 방식

- 자바스크립트 객체는 프로퍼티 키를 인덱스로 하는 <b>해시 테이블(hash table)</b>이라고 생각할 수 있다.
  - 대부분의 자바스크립트 엔진은 해시 테이블과 유사하지만 높은 성능을 위해 일반적인 해시 테이블보다 나은 방법으로 객체를 구현한다.
- 자바스크립트는 자바, C++ 언어와 달리 클래스 없이 객체를 생성할 수 있으며 객체가 생성된 이후라도 동적으로 프로퍼티와 메서드를 추가할 수 있따.
  - 이는 사용하기 매우 편리하지만 성능 면에서는 이론적으로 클래스 기반 객체지향 프로그래밍 언어의 객체보다 생성과 프로퍼티 접근에 비용이 더 많이 드는 비효율적인 방식이다.
- 따라서 V8 자바스크립트 엔진에서는 프로퍼티에 접근하기 위해 동적 탐색대신 <b>히든 클래스(hidden class)</b>라는 방식을 사용해 성능을 보장한다.

<br>

### (2) 자바스크립트의 히든 클래스(hidden class)

#### :round_pushpin: 동적 탐색(dynamic lookup)

- 자바스크립트는 동적 타이핑 언어로 코드를 실행할 때의 <u>상황에 따라 데이터 타입이 정해진다.</u>

  - 그러다보니 객체의 프로퍼티에 접근하는 속도 면에서는 정적 타이핑 언어의 코드와 비교했을 때 불리해질 수 있다.

- 정적 타이핑 언어에서는 가변길이 배열과 같이 동적인 데이터 타입을 사용하여 프로퍼티의 메모리 오프셋을 컴파일 시에 결정할 수 있찌만 동적 타이핑 언어에서는 프로퍼티를 선언했을 때의 프로퍼티의 데이터 타입이나 순서가 실제로 프로퍼티 값을 접근할 때 달라질 수 있어 프로퍼티의 메모리 오프셋을 컴파일할 때 결정하는 것은 불가능하다.

  - 참고로 여기서 오프셋이란 메모리 주소를 탐색할 때 기준 주소에서 얼만큼 떨어져 있는지 나타내는 주소를 가리킬 때 쓰인다.

- 이로 인해 동적 타이핑 언어인 자바스크립트에서는 <b>동적 탐색(dynamic lookup)</b>으로 프로퍼티 값을 읽어야 한다.

- 정적 타이핑 언어와 동적 타이핑 언어의 차이

  ![hiddenclass](https://user-images.githubusercontent.com/52685250/152634284-36fda41c-ada0-440c-9268-498507139202.png)

  - 정적 타이핑 언어(왼쪽)는 오프셋 값만으로도 모든 멤버 변수에 접근할 수 있지만, 동적 타이핑 언어(오른쪽)는 수행 중간에 필드 구조가 변경될 수 있으므로 기본적으로 프로퍼티와 값 세트를 모든 객체가 들고 있어야 한다.
  - 동적 타이핑 언어는 언제 프로퍼티와 값이 바뀔 지 모르므로 계속해서 모든 테이블을 들고 다녀야 한다.
  - 이로 인해 중복된 데이터로 인한 메모리적인 측면 뿐만 아니라 성능까지도 문제가 생길 수 밖에 없다.

<br>

#### :round_pushpin: V8 자바스크립트 엔진에서 동적 탐색을 회피하는 방법 - 히든 클래스(hidden class)

> V8은 히든 클래스를 이용하여 동적 탐색(Dynamic Lookup)을 회피하고 있다.
>
> 한마디로 말하자면, 프로퍼티가 바뀔 때 각각 그 프로퍼티의 오프셋을 업데이트한 뒤 그 값을 가지고 있는 방식이다.

##### :bulb: hidden class 특징

- 반드시 하나의 객체마다 부여된다.
- 각 프로퍼티에 대해 <b>오프셋 정보</b>를 가지고 있다.
- 객체에 프로퍼티가 <b>추가, 수정, 삭제되면 새로운 hidden class</b>가 만들어지며, 이는 기존 hidden class 정보에 추가로 업데이트된 정보를 가지고 있게 된다.
- 위 과정에서 원래의 hidden class는 참조해야되는 hidden class 정보(<b>전환정보</b>)가 추가된다.

<br>

##### :bulb: hidden class 동작 방식

```javascript
// STEP 1

var obj = {};
```

- 우선 아무것도 없는 빈 객체를 생성하자.
- 객체를 생성하면 무조건 히든 클래스(이 단계의 히든 클래스르 `C0` 이라고 하자.)가 생성되고 이 객체가 `C0` 히든 클래스를 참조하게 된다.
- 이 때 `obj` 객체가 프로퍼티를 갖고 있지 않으므로 히든 클래스에는 아직 아무런 정보도 없는 상태이다.

```javascript
// STEP 2

var obj = {};
obj.x = 1;
```

- 위와 같이 객체를 생성한 후 `obj.x` 프로퍼티에 값을 대입해보자.
- 객체에 프로퍼티를 추가했으므로 새로운 히든 클래스 `C1`이 생성되고 `x` 프로퍼티에 대한 오프셋 값을 갖는다.
- `obj` 객체는 원래 `C0` 히든 클래스를 참조했었는데 새로운 프로퍼티가 생겼으므로 `C1` 히든 클래스를 참조하게 된다.
- 그리고 중요한 포인트는 `x` 프로퍼티를 추가하면 참조하는 히든 클래스가 `C1`으로 전환(transition)된다는 정보가 `C0` 클래스에 추가된다.

```javascript
// STEP 3

var obj = {};
obj.x = 1;
obj.y = 2;
```

- 이번에는 `obj.y` 프로퍼티에 값을 대입해보자.
- 이때도 새로운 프로퍼티가 추가되므로 히든 클래스 `C2` 가 생성되고 `y` 프로퍼티에 대한 오프셋 값을 `C2`에서 가지게 된다.
- 그리고 `obj` 객체에 대응하는 히든 클래스가 `C1`에서 `C2`로 바뀐다. (<u>`C1` 히든 클래스에 `y` 프로퍼티의 오프셋 값을 추가하는 것이 아님!</u>)
- 마찬가지로 `C1` 히든 클래스에는 `y` 프로퍼티를 추가하면 참조하는 히든 클래스가 `C2`로 전환된다는 정보가 추가된다.

```javascript
// STEP 4

var obj = {};
obj.x = 1;
obj.y = 2;
console.log(obj.y);
```

- 만약 위와 같이 `obj.y` 프로퍼티에 접근한다면 내부적으로 어떻게 동작하게 될까?
- `obj` 객체와 연결되어 있는 히든 클래스는 `C2`가 되며 여기에 적혀 있는 `y` 프로퍼티의 오프셋을 이용해서 `y`의 값을 참조하게 된다.
- 이것이 바로 V8이 사용하는 동적 탐색을 회피하는 히든 클래스 방식이다.

<br>

##### :bulb: hidden class의 효율을 높여주는 전환 정보

- 위 내용에서 '전환 정보'라는 용어가 등장했다.
- 중간에 객체의 필드 구조가 변한다면 이 테이블을 참조하여 객체가 다른 히든 클래스로 옮겨가거나 혹은 새로 생성하게 된다.

```javascript
function foo(value) {
  this.x = value;
}

var a = new foo(1); // (a)
var b = new foo(2); // (b)
```

- 위 코드에서 (a) 부분이 실행되는 시점에서는 다음과 같은 정보가 존재하며 `a` 객체는 `C1` 히든 클래스를 참조하고 있는 상태다.
  - 히든 클래스 `C0`
    - 프로퍼티의 오프셋 값 없음
    - `x` 프로퍼티를 추가하면 참조하는 히든 클래가 `C1` 으로 전환된다. => <b><u>전환정보</u></b>
  - 히든 클래스 `C1`
    - `x` 프로퍼티의 오프셋 값
- 이어서 (b) 부분에서 `b` 객체가 생성되는데 이 과정에서 `b` 객체가 `C0` 클래스를 참조했을 때 위에서 살펴본 전환정보가 존재하므로 이로 인해 `x` 프로퍼티를 추가할 때 새로운 히든 클래스를 생성하지 않고 `C1` 클래스를 참조하여 자기 자신과 `C1` 클래스를 연결시킨다.
- 이렇게 해서 히든 클래스를 쓸데 없이 늘리지 않으면서 오프셋을 효율적으로 관리하는 목적을 달성한다.

<br>

##### :bulb: Inline Caching

- Inline Caching의 기본 개념

  - V8 엔진의 최적화 포인트는 최소한의 hidden class를 만드는 것이다.
  - 메모리 소모는 줄일지언정, 프로퍼티를 변경할 때마다 hidden class가 생성된다.
  - 이를 방지하기 위해 <b>Inline Caching</b> 기법을 사용한다.
  - 객체 필드에 접근할 때 hidden class를 사용하는데 이 때 우리가 얻고 싶은 것은 필드의 오프셋 값이다.
    - Inline Caching을 통해서 이 <b>오프셋 값을 캐싱</b>한다는 의미이다.
  - 자바스크립트 엔진에서 Inline Caching의 전제 조건
    - 동적인 언어라고 해봤자 실제로는 안바뀌는데 더 많다.
    - 성능을 빠르게 하려면 딴 거 다 필요없고 <b>루프</b>를 돌려라.
  - 즉, hidden class가 있다고 해도 유지 중인 오프셋을 사용하여 메모리를 참조하는 작업은 여전히 남아있는데, 이 작업이 <b>같은 조건 하에 여러 번 반복</b>될 때 그 결과를 캐싱해서 전달하자는 것이 Inline Caching의 핵심이다.

- Inline Caching의 적용 사례

  ```javascript
  for (var i = 0; i < 10; i += 1) {
    arr[i].x = i;
  }
  ```

  - 위 코드에서 `.x` 부분이 Inline Caching이 일어나는데 자세히 살펴보자.

  - 우선 `i = 0` 일 때는 캐싱된 값이 없으므로 `arr[0]`의 내부 구조와 `arr[0].x` 오프셋 값이 캐싱될 준비를 마친다.

  - 그 후 두 번째(`i = 1`)부터 마지막(`i = 9`)까지는 캐싱된 오프셋 값을 바로 쓸 수 있게된다.

    - 정확하게는 두 번째 수행부터 캐싱하게 된다.

  - 단, `arr[1]` 부터 `arr[9]`까지 <b>모두 같은 필드 구조를 가지고 있어야만 성립이 된다.</b>

    ```javascript
    const arr = [
      {
        x: 1,
        y: 2,
      },
      {
        x: 3,
        y: 4,
      },
    ];
    ```

    - 다른 객체가 이것저것 섞여 있던가 `x` 프로퍼티가 없어 새롭게 추가해야하는 상황이라면 이런 경우에는 Inline Caching의 혜택을 받지 못하여 오히려 손해를 보게 된다.
    - 즉, 위 예시 `arr`  처럼 동일한 필드 구조를 가지고 있어야 한다.

- 추가로 자바스크립트 엔진에서는 다양한 방식으로 최적화를 이뤄가고 있는데 [자바스크립트 엔진의 최적화 기법 (1) - JITC, Adaptive Compilation](https://meetup.toast.com/posts/77) 게시글을 통해서 자세한 내용을 파악하는 시간을 가져보는 것도 좋을 것 같다.

  - 만약 시간이 없다면 이 게시글에서 '4. Adaptive JIT Compilation', '5. 결론' 단락 부분만 읽어도 좋다.
  - 특히 마지막에 나와 있는 것처럼 성능이 좋은 자바스크립트 코드를 만들고 싶다면 정적 타이핑 언어처럼 코드를 작성하는 것이 좋다는 것에 주목하자.
  - `number ` 타입이면 `number`에 맞게 `string` 타입이면 `string` 타입으로 고정하여 코드를 작성하는 것이 성능적인 측면에서도 좋다. (특히 array!)

<br>

#### :round_pushpin: Reference

- [자바스크립트 엔진의 최적화 기법 (2) - Hidden class, Inline Caching](https://meetup.toast.com/posts/78)
- [V8의 히든 클래스 이야기](https://engineering.linecorp.com/ko/blog/v8-hidden-class/)
- [[개념정리] 자바스크립트 엔진 v8이 사용하는 Hidden Class, Inline Cacahing](https://crmrelease.tistory.com/79)

<br>

### (3) 변경 가능한 값

- 원시 값을 할당한 변수를 참조하면 메모리에 저장되어 있는 원시 값에 접근한다.
- 하지만 객체를 할당한 변수를 참조하면 메모리에 저장되어 있는 <b>참조 값(reference value)</b>을 통해 실제 객체에 접근한다.
- 객체의 할당 과정

```javascript
var user = {
  name: 'wally',
};

console.log(user); // {name: "wally"}
```

<img src="https://user-images.githubusercontent.com/52685250/152636680-60cae743-f82b-41b8-b7c8-e9fe010c6873.JPG" width="600">

- 변경 가능한 객체
  - 원시 값과 다르게 객체는 재할당 없이 객체를 직접 변경할 수 있다.
  - 아래 예시 코드와 같이 프로퍼티를 동적으로 추가할 수도 있고 프로퍼티 값을 갱신할 수도 있으며 프로퍼티 자체를 삭제할 수도 있다.
  - 객체는 원시 값과 달리 변수에 재할당을 하지 않았으므로 객체를 할당한 변수의 참조 값을 아래 그림과 같이 변경되지 않는다.

```javascript
var user = {
  name: 'wally',
};

// 프로퍼티 값 갱신
user.name = 'wallywally';

// 프로퍼티 동적 생성
user.zipCode = 12345;

console.log(user); // {name: "wallywally", zipCode: 12345}
```

<img src="https://user-images.githubusercontent.com/52685250/152636758-ee1a101e-c750-45c7-8423-75ae57ba12fc.JPG" width="600">

- 객체 복사시 trade off

  - 객체를 생성하고 관리하는 방식은 매우 복잡하며 비용이 많이 드는 일이다.
  - 객체를 변경할 때마다 원시 값처럼 이전 값을 복사해서 새롭게 생성한다면 명확하고 신뢰성이 확보되겠지만 객체는 크기가 매우 클 수도 있고, 원시 값처럼 크기가 일정하지도 않으며, 프로퍼티 값이 객체일 수도 있어 복사해서 생성하는 비용이 많이 든다.
  - 따라서 메모리를 효율적으로 사용하기 위해, 그리고 객체를 복사해 생성하는 비용을 절약하여 성능을 향상시키기 위해 객체는 변경 가능한 값으로 설계되어 있다.
  - 메모리 사용의 효용성과 성능을 위해 어느 정도의 구조적인 단점을 감안한 설계라고 할 수 있다.

- 객체의 얕은 복사(shallow copy) vs 깊은 복사(deep copy)

  - 얕은 복사(shallow copy)는 객체의 <b>한 단계까지만 복사</b>하는 것을 말하고 깊은 복사(deep copy)는 객체에 <b>중첩되어 있는 객체까지 모두 복사</b>하는 것을 말한다.
    - 얕은 복사와 깊은 복사로 생성된 객체는 원본과는 다른 객체다.
      - 즉, 원본과 복사본은 참조 값이 다른 별개의 객체다.

  - 하지만 얕은 복사는 객체에 중첩되어 있는 객체의 경우 <b>참조 값을 복사</b>하고 깊은 복사는 객체에 중첩되어 있는 객체까지 <b>모두 복사해서 원시 값처럼 완전한 복사본을 만든다</b>는 차이가 있다.

---

### :heavy_plus_sign: 객체의 얕은 복사(shallow copy) 방법

#### :round_pushpin: `Object.assign()` 메서드 - [MDN 공식 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

```javascript
const obj = {
  x: 1,
  y: {
    z: 2,
  },
};

const newObj = Object.assign({}, obj);

newObj.x = 0;
newObj.y.z = 3;

console.log(obj); // {x: 1, y: {z: 3}}
console.log(newObj); // {x: 0, y: {z: 3}}
```

<br>

#### :round_pushpin: 전개 연산자(Spread Operator) - [MDN 공식 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

```javascript
const obj = {
  x: 1,
  y: {
    z: 2,
  },
};

const newObj = { ...obj };

newObj.x = 0;
newObj.y.z = 3;

console.log(obj); // {x: 1, y: {z: 3}}
console.log(newObj); // {x: 0, y: {z: 3}}
```

<br>

### :heavy_plus_sign: 객체의 깊은 복사(deep copy) 방법

#### :round_pushpin: JSON 객체 메소드 이용 - `JSON.stringify()` - [MDN 공식 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify), `JSON.parse()` - [MDN 공식 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)

```javascript
const obj = {
  x: 1,
  y: {
    z: 2,
  },
};

const newObj = JSON.parse(JSON.stringify(obj));

newObj.x = 0;
newObj.y.z = 3;

console.log(obj); // {x: 1, y: {z: 2}}
console.log(newObj); // {x: 0, y: {z: 3}}
```

- 하지만 이 방법은 치명적인 단점 두 가지가 있다.
  - 다른 방법에 비해 성능이 느리다.
  - `JSON.stringify()` 메서드는 함수를 만났을 때 `undefined`로 처리한다.

```javascript
const obj = {
  x: 1,
  y: {
    z: 2,
  },
  f1: function() {
    return this.x;
  }
};

const newObj = JSON.parse(JSON.stringify(obj));

console.log(newObj); // {x: 1, y: {z: 2}}
console.log(newObj.f1); // undefined
```

<br>

#### :round_pushpin: lodash 라이브러리의 `cloneDeep()` 메서드 - [공식 문서(4.17.15 버전)](https://lodash.com/docs/4.17.15#cloneDeep)

> 참고로 커스텀 재귀 함수로 구현할 수 있지만 많은 개발자들에 의해 검증되어 있고 오래동안 쓰여온 lodash의 `cloneDeep()` 메서드 사용하는 것을 권장한다.

```bash
npm i lodash
```

```javascript
// tree shaking 기법을 이용해서 lodash의 메서드 중 사용할 메서드만 가져오는 방식
import cloneDeep from 'lodash/cloneDeep';

const obj = {
  x: 1,
  y: {
    z: 2,
  },
  f1: function() {
    return this.x;
  }
};

const newObj = cloneDeep(obj);

newObj.x = 0;
newObj.y.z = 3;

console.log(obj); // {x: 1, y: {z: 2}, f1: f()}
console.log(obj.f1()); // 1
console.log(newObj); // {x: 0, y: {z: 3}, f1: f()}
console.log(newObj.f1()); // 0
```

<br>

### (4) 참조에 의한 전달

- 자바스크립트의 객체는 원시 값과는 다르게 여러 개의 식별자가 하나의 객체를 공유할 수 있다.
- 이로 인한 부작용이 존재하는데 이미 위에서 객체의 얕은 복사와 깊은 복사를 통해서 살펴보았지만 다시 한 번 더 정리해보자.

```javascript
var user = {
  name: 'wally',
};

// 참조 값을 복사(얕은 복사)
var copyUser = user;
```

- 객체를 가리키는 변수(원본, `user`)를 다른 변수(사본, `copyUser`)에 할당하면 원본의 <b>참조 값이 복사되어 전달</b>되는데 이를 <b>참조에 의한 전달</b>이라 한다.

![참조에 의한 전달](https://user-images.githubusercontent.com/52685250/152638691-c9ef9494-3e8e-43ba-88ea-22aa9290b137.JPG)

- 위 그림처럼 원본 `user`를 사본 `copyUser`에 할당하면 원본 `user`의 참조 값을 복사해서 `copyUser`에 저장한다.
  - 이때 원본 `user`와 사본 `copyUser`는 저장된 메모리 주소는 다르지만 동일한 참조 값을 갖는다.
  - 다시 말해, 두 변수 모두 동일한 객체를 가리키며 이는 <b>두 개의 식별자가 하나의 객체를 공유</b>한다는 것을 의미한다.
- 이러한 특성으로 인해 어느 한 쪽에서 객체를 변경하면 서로 영향을 주고받는다.

```javascript
var user = {
  name: 'wally',
};

var copyUser = user;

console.log(user === copyUser); // true

copyUser.name = 'wallywally';
copyUser.zipCode = 12345;

console.log(user); // {name: "wallywally", zipCode: 12345}
console.log(copyUser); // {name: "wallywally", zipCode: 12345}
```

- 결국 '값에 의한 전달'과 '참조에 의한 전달'은 식별자가 기억하는 <b>메모리 공간에 저장되어 있는 값을 복사해서 전달한다</b>는 면에서 동일하다.
  - 다만 식별자가 기억하는 메모리 공간, 즉 변수에 저장되어 있는 값이 <b>원시 값이냐 참조 값이냐의 차이</b>만 있을 뿐이다.

```javascript
var user1 = {
  name: 'wally',
};

var user2 = {
  name: 'wally',
};

console.log(user1 === user2); // (1)
console.log(user1.name === user2.name); // (2)
```

| 문제 | 결과    | 이유                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| (1)  | `false` | `user1` 변수와 `user2` 변수가 가리키는 객체는 비록 내용은 같지만 다른 메모리에 저장된 별개의 객체이기 때문이다. |
| (2)  | `true`  | 프로퍼티 값을 참조하는 `user1.name`, `user2.name`은 값으로 평가될 수 있는 표현식이고 모두 원시 값 `'wally'`로 평가되기 때문이다. |
