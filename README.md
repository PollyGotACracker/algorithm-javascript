# Algorithm-Javascript

## 학습

- [김태원, 자바스크립트 알고리즘 문제풀이 입문(코딩테스트 대비)](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4)

## 공부

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
