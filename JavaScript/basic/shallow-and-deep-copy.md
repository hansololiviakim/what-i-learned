# 얕은 복사(shallow copy)와 깊은 복사(deep copy)

## 🗂️ 목차

- 💡 [요약 정리](#-요약-정리)

- 📚 [원시 값 할당 방식](#-원시-값-할당-방식)

- 📚 [객체 값 할당 방식](#-객체-값-할당-방식)

- 📚 [객체 얕은 복사](#-객체-얕은-복사)

- 📚 [객체 깊은 복사](#-객체-깊은-복사)

- 📎 [참고 자료](#-참고-자료)

<br /><br />

## 💡 요약 정리

> <br />객체의 얕은 복사와 깊은 복사는 **객체를 복사하는 방식의 차이**에서 비롯된다.
> <br />얕은 복사는 **객체의 참조 값**(메모리 주소)만 복사하는 방식으로, 복사본과 원본이 동일한 객체를 가리키므로 **중첩 객체를 변경하면 원본에도 영향**을 미친다.
> <br />반면, 깊은 복사는 **내부의 중첩된 객체까지 새로운 메모리 공간에 복사**하여 **독립적인 객체**를 생성하므로, 변경해도 서로 영향을 주지 않는다.
> <br />따라서 성능과 메모리 효율을 고려하여 적절한 복사 방식을 선택해야 한다.<br /><br />

<br /><br />

## 📚 원시 값 할당 방식

> <br />원시 값은 값 자체가 메모리에 저장되며, 새로운 값을 할당할 때마다 새로운 메모리 공간을 사용한다.
> <br />따라서 원시 값을 다른 변수에 할당하면 값이 복사되며, 원본과 독립적으로 동작한다.
> <br />이는 **'값에 의한 복사' 방식으로, 변수 간 영향을 주지 않는다.**<br /><br />

<br />

### 예시

```javascript
let a = 5;
let b = a; // 값 자체가 복사됨
a = 10;

console.log(a); // 10
console.log(b); // 5 (변하지 않음)
```

- 원시 값을 다른 변수에 할당하면 **새로운 값이 복사**되므로, 원본과 독립적으로 동작한다.

<br /><br />

## 📚 객체 값 할당 방식

> <br />객체 값은 메모리에 **참조(reference) 값**으로 저장되며, 변수에는 객체의 메모리 주소가 저장된다.
> <br />따라서 객체를 다른 변수에 할당하면 값이 복사되는 것이 아니라 같은 객체를 가리키는 **참조 값만 복사**된다.
> <br />이로 인해 한 변수를 통해 객체를 변경하면, 같은 객체를 참조하는 다른 변수에도 영향을 미친다.<br /><br />

<br />

### 예시

```javascript
let obj_a = { name: "john" };
let obj_b = obj_a; // 참조 값 복사 (같은 객체를 가리킴)

obj_a.name = "anna"; // 객체 속성 변경

console.log(obj_a.name); // anna
console.log(obj_b.name); // anna (같은 객체를 참조하므로 변경됨)
```

- 같은 객체를 참조하므로 `obj_a.name`을 변경하면 `obj_b.name`도 영향을 받는다.

<br /><br />

## 📚 객체 얕은 복사

> <br />얕은 복사는 객체의 참조 값(메모리 주소)만 복사하는 방식이므로, 원본과 복사본이 동일한 객체를 참조하게 된다.
> <br />따라서 복사한 객체의 중첩된 속성을 변경하면 원본에도 영향을 미친다.<br /><br />

<br />

### 구현 방법

- 스프레드 연산자 `({ …obj })`

- `Object.assign()` 메서드

<br />

### 예시

```javascript
let obj1 = { name: "John", details: { age: 30 } };
let obj2 = { ...obj1 };

obj2.details.age = 40;

console.log(obj1.details.age); // 40 (원본도 변경됨)
```

<br />

### [참고] `Object.assign()`과 `{ ...obj }`의 차이점

- `{ ...obj }`와 `Object.assign()`은 기본적으로 동일한 방식으로 얕은 복사를 수행한다.

- 하지만, `Object.assign()`은 **getter를 무시하고, 상속된 프로퍼티를 복사할 수 있다.**

<br /><br />

## 📚 객체 깊은 복사

> <br />깊은 복사는 원본 객체뿐만 아니라 **내부의 중첩된 객체까지 새로운 메모리 공간을 할당하여 복사**하는 방식이다.
> <br />이로 인해 복사된 객체는 **원본과 완전히 독립적으로 동작**하며, 어느 한쪽을 수정해도 서로 영향을 주지 않는다.<br /><br />

<br />

### 구현 방법

- **재귀적으로 모든 하위 객체를 복사**하는 직접 구현 방법

- `structuredClone()` (최신 브라우저에서만 지원)

- `JSON.parse(JSON.stringify(obj))` (일부 데이터 타입 문제 있음)

- Lodash의 `_.cloneDeep()` 메서드 활용

<br />

### 예시

```javascript
let obj1 = { name: "John", details: { age: 30 } };
let obj2 = JSON.parse(JSON.stringify(obj1)); // 깊은 복사 (JSON 방식)

obj2.details.age = 40;

console.log(obj1.details.age); // 30 (원본 유지)
console.log(obj2.details.age); // 40 (복사본만 변경)
```

<br />

### [참고] `structuredClone()`의 장점

- `structuredClone()`은 `JSON.parse(JSON.stringify(obj))`와 달리 `undefined`, `Date`, `RegExp` 같은 객체도 정상적으로 복사할 수 있다.

- 하지만, **최신 브라우저에서만 사용 가능하므로 호환성을 고려**해야 한다.

<br /><br />

## 📎 참고 자료

- [참조에 의한 객체 복사](https://ko.javascript.info/object-copy)

- [깊은 복사](https://developer.mozilla.org/ko/docs/Glossary/Deep_copy)
