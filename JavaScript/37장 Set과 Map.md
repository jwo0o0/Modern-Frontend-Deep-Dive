# 37. Set과 Map

## 37.1 Set

💡 **Set 객체**: 중복되지 않는 유일한 값들의 집합

### 배열과 Set 객체의 차이점

|                                      | 배열 | Set 객체 |
| ------------------------------------ | ---- | -------- |
| 동일한 값을 중복하여 포함할 수 있다. | O    | X        |
| 요소 순서에 의미가 있다.             | O    | X        |
| 인덱스로 요소에 접근할 수 있다.      | O    | X        |

- Set 객체의 특성은 수학적 집합의 특성과 일치
- 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있음

### Set 객체의 생성

```jsx
const set = new Set();
console.log(set); // Set(0) {}
```

**Set 생성자 함수**: 이터러블을 인수로 전달받아 Set 객체를 생성

\*이 때 중복된 값은 Set 객체에 요소로 저장되지 않음

```jsx
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set("hello");
console.log(set2); // Set(4) {'h', 'e', 'l', 'o'}
```

### 배열의 중복요소 제거에 활용

```jsx
const uniq = (array) => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

### 요소 개수 확인

```jsx
const { size } new Set([1, 2, 3, 3]);
console.log(size); // 3
```

- `Set.prototype.size` 프로퍼티로 요소 개수 확인

### 요소 추가

```jsx
const set = new Set();
console.log(set);

set.add(1).add(2); // 연속적으로 호출 가능
console.log(set); // Set(2) {1, 2}
```

- `Set.prototype.add` 메서드로 객체에 요소를 추가

```jsx
const set = new Set();

console.log(NaN === NaN); // false
set.add(NaN).add(NaN);
```

- **NaN과 NaN을 같다고 평가**하여 중복 추가를 허용하지 않음
- **0과 -0을 같다고 평가**하여 중복 추가를 허용하지 않음

### 요소 존재 여부 확인

```jsx
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

- `Set.prototype.has` 메서드로 존재 여부를 확인할 수 있음

### 요소 삭제

```jsx
const set = new Set([1, 2, 3]);

set.delete(2);
```

- `Set.prototype.delete` 메서드로 요소를 삭제할 수 있음
- 인덱스가 아니라 **삭제하려는 요소값을 인수로 전달**
- add 메서드와 달리 연속적으로 호출할 수 없음

### 요소 일괄 삭제

```jsx
const set = new Set([1, 2, 3]);
set.clear();
console.log(set); // Set(0) {}
```

- `Set.prototype.clear` 메서드 사용

### 요소 순회

```jsx
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
```

- Set.prototype.forEach 메서드 사용
- 콜백 함수 내부에서 this로 사용될 객체(옵션)을 인수로 전달
  - 첫번째 인수: 현재 요소 값
  - 두번째 인수: 현재 요소 값(배열과 같이 인덱스를 갖지 않음)
  - 세번째 인수: 현재 순회 중인 Set 객체 자체
- **Set 객체는 이터러블**이므로 for … of 문으로 순회할 수 있고, 스프레드 문법과 배열 디스트럭처링 대상이 될 수 있음

## 37.2 Map

💡 **Map 객체: 키와 값의 쌍으로 이루어진 컬렉션**

### 배열과 Map 객체의 비교

|                        | 객체                    | Map 객체              |
| ---------------------- | ----------------------- | --------------------- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값     | 객체를 포함한 모든 값 |
| 이터러블               | X                       | O                     |
| 요소 개수 확인         | Object.keys(obj).length | map.size              |

### Map 객체의 생성

```jsx
const map = new Map();
console.log(map); // Map(0) {}
```

- Map 생성자 함수: **이터러블을 인수로 전달받아 Map 객체를 생성**
  - 이때 이터러블은 **키와 값의 쌍으로 이루어진 요소로 구성**되어야 함

```jsx
const map1 = new Map(["key1", "value1"], ["key2", "value2"]);
console.log(map1); // Map(2) {'key1' => 'value1', 'key2 => 'value2'}
```

- 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 덮어써짐

### 요소 개수 확인

```jsx
const { size } = new Map(["key1", "value1"], ["key2", "value2"]);
console.log(size);
```

- `Map.prototype.size` 프로퍼티 사용
- `size` 프로퍼티는 getter 함수만 존재하는 접근자 프로퍼티이므로 **숫자를 할당하여 Map 객체의 요소 개수를 변경할 수 없음**

### 요소 추가

```jsx
const map = new Map();
console.log(map);

map.set("key1", "value1");
console.log(map); // Map(1) {'key1' => 'value1'}
```

- `Map.prototype.set` 메서드 사용

```jsx
const map = new Map();

map
	.set('key1', 'value1');
	.set('key2', 'value2');
```

- set 메서드를 연속적으로 호출할 수 있음
- 중복된 키를 갖는 요소를 추가하면 값이 덮어써짐
- Map 객체는 NaN과 NaN을 같다고 평가, +0과 -0을 같다고 평가

```jsx
const map = new Map();
const lee = { name: "lee" };
const kim = { name: "kim" };

map.set(lee, "developer").set(kim, "designer");
```

- 객체와 다르게 키 타입에 제한이 없음
  - 객체를 포함한 모든 값을 키로 사용 가능

### 요소 취득

```jsx
console.log(map.get(lee)); // developer
console.log(map.get("key")); // undefined
```

- `Map.prototype.get` 메서드로 키를 전달하면 value를 반환

### 요소 존재 여부 확인

```jsx
console.log(map.has(lee)); // true
console.log(map.has("key")); // false
```

- `Map.prototype.has` 메서드로 확인

### 요소 삭제

```jsx
map.delete(kim);
```

- `Map.prototype.delete` 메서드로 삭제
- 존재하지 않는 키로 삭제시 에러 없이 무시

### 요소 일괄 삭제

```jsx
map.clear();
console.log(map); // Map(0) {}
```

- `Map.prototype.clear` 메서드 사용

### 요소 순회

```jsx
map.forEach((v, k, map) => console.log(v, k, map));
```

- `Map.prototype.forEach` 메서드 사용
- 콜백 함수에서 3개의 인수를 전달받음
  - 첫번째 인수: 현재 순회중인 요소 값
  - 두번째 인수: 현재 순회중인 요소 키
  - 세번째 인수: 현재 순회 중인 Mp 객체 자체
- Map 객체는 이터러블이므로 for…of 문으로 순회 가능, 스프레드 문법과 배열 디스트럭처링 할당 가능

| Map.prototype.keys    | 요소 키를 값으로 갖는 이터러블이면서 이터레이터인 객체를 반환           |
| --------------------- | ----------------------------------------------------------------------- |
| Map.prototype.values  | 요소 값을 값으로 갖는 이터러블이면서 이터레이터인 객체를 반환           |
| Map.prototype.entries | 요소 키와 요소 값을 값으로 갖는 이터러블이면서 이터레이터인 객체를 반환 |
