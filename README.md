# Algorithm-Javascript

## 학습

- [김태원, 자바스크립트 알고리즘 문제풀이 입문(코딩테스트 대비)](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4)

## 공부

- [note 폴더](/note)
- [wikidocs.net: 다빈치코딩 알고리즘](https://wikidocs.net/book/10280)
- [geeksforgeeks.org: DSA for Beginners](https://www.geeksforgeeks.org/complete-guide-to-dsa-for-beginners/)

### `Array.slice()` 와 `Array.splice()`

- `slice()` 와 달리 `splice()` 는 원본 배열을 변경한다.
- 두 번째 매개변수로 `slice()` 는 end(반환받을 마지막 요소의 index + 1. end 위치의 이전 요소까지 잘라냄),  
  `splice()` 는 deleteCount(배열에서 제거할 요소의 수)를 받는다.
- `splice()` 는 세 번째 매개변수부터 삭제한 위치부터 추가할 요소를 받는다.

### `String.match()` 와 `String.matchAll()`

- `match()` 는 배열, `matchAll()` 은 RegExpStringIterator 반환
- `match()` 는 `g` 플래그 없을 경우 일치하는 첫 부분만 반환, `matchAll()` 은 `g` 플래그 무조건 필요
- `match()` `matchAll()` 모두 두 번째 매개변수는 콜백함수를 받는다.

```js
function replacer(match, p1, p2, …, pN, offset, string, groups) {
  return replacement;
}
/*
match: 일치하는 문자열
p1, p2, …, pN: 정규표현식에서 캡처 그룹 () 으로 찾은 각 문자열
offset: 검사한 전체 문자열에서 match 문자열의 인덱스
string: 검사한 전체 문자열
groups: 이름이 있는 캡처 그룹 (?<name>...) 객체: groups.name
*/
```

### `Math.floor()` 와 `parseInt()`

- 값이 음수일 경우 반환값이 달라진다.
  - `Math.floor()` 는 소수점에서 내림한다.
  - `parseInt()` 는 단순히 소수점 이하 숫자를 없애므로 결과적으로 올림한다.

```js
Math.floor(-14.5); // -15
parseInt(-14.5); // -14
```

### double tilde 연산자 `~~`

- 값이 양수일 경우 `Math.floor()`, 음수일 경우 `Math.ceil()` 과 같은 결과를 얻는다.  
  cf. tilde 연산자 `~`: NOT 비트연산

```js
Math.floor(14.5); // 14
Math.ceil(14.5); // 15
~~14.5; // 14 :양수, 내림
Math.floor(-14.5); // -15
Math.ceil(-14.5); // -14
~~-14.5; // -14 :음수, 올림
```

### while 문 조건식에 메서드 사용

```js
// section6 2번 문제
while (stack.pop() !== "(");
```

- `stack.pop()` 에서 `(` 가 나올 때까지(false) `stack.pop()` 반복 호출

### 반복문에서 변경된 다차원 배열을 콘솔에 출력

- 반복문에서 다차원 배열을 변경하고, 각각 변경된 배열을 콘솔에 출력하고자 하는 경우,  
  가장 마지막으로 변경된 배열만 반복 출력되는 문제

```js
// section6 3번 문제 풀이 확인
console.log(JSON.parse(JSON.stringify(board)));
console.table(JSON.parse(JSON.stringify(board)));
```

- `JSON.parse()` 와 `JSON.stringify()` 를 사용하여 깊은 복사를 수행하면 정상적으로 출력된다.
- `console.log()` 를 통해 다차원 배열을 출력할 때 값이 아닌 참조(reference)가 출력되기 때문이다.
- 반복문에서 다차원 배열의 값을 변경하면 모든 반복에서 같은 배열을 참조하게 되며,  
  반복이 끝나고 나서야 실제 변경된 값이 반영되어 콘솔에 출력된다.
- 단, index 를 사용해 특정 위치의 값을 콘솔에 출력하는 것은 참조 문제가 발생하지 않는다.
