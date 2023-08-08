# Algorithm-Javascript

## 학습

- [김태원, 자바스크립트 알고리즘 문제풀이 입문(코딩테스트 대비)](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4)

## 참고

- [wikidocs.net: 다빈치코딩 알고리즘](https://wikidocs.net/book/10280)

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

### Two Pointers Algorithm;투 포인터 알고리즘

- 슬라이딩 윈도우 알고리즘은 범위가 고정되어 있지만, 투 포인터 알고리즘은 _범위가 동적으로 변한다_.
- 이중반복문의 시간복잡도는 O(n^2) 이므로 배열 크기가 커질수록 연산량이 제곱으로 증가하는 반면,  
  투 포인터 알고리즘의 시간복잡도는 O(n) 이므로 더 효율적이다.
- 요소의 합계나 평균이 k 인 연속부분수열을 모두 찾을 때

#### 배열의 처음에서 시작하여 마지막으로 이동하는 경우

1. 필요 시 배열 정렬
2. 두 포인터 모두 0 번째로 초기화
3. 계산한 값이 k 보다 작다면 끝 포인터를 오른쪽으로 이동
4. 계산한 값이 k 보다 크다면 시작 포인터를 오른쪽으로 이동

```js
// section5 3번 문제
const solution = (m, arr) => {
  let answer = 0,
    sum = 0;
  let lt = 0;

  for (let rt = 0; rt < arr.length; rt++) {
    // sum 이 m 보다 작다면
    // 끝 포인터를 오른쪽으로 이동
    // 그리고 끝 포인터 값을 sum 에 합산
    sum += arr[rt];
    if (sum === m) answer++;
    while (sum >= m) {
      // sum 이 m 보다 크거나 같다면
      // 시작 포인터 값을 sum 에서 뺀 후
      // 시작 포인터를 오른쪽으로 이동
      sum -= arr[lt++];
      if (sum === m) answer++;
    }
  }
  return answer;
};

let a = [1, 2, 1, 3, 1, 1, 1, 2];
console.log(solution(6, a));
```

#### 배열의 양 끝에서 시작하여 가운데로 이동하는 경우

1. 필요 시 배열 정렬
2. 시작 포인터와 끝 포인터를 각각 0 과 배열 크기 - 1 번째로 초기화
3. 계산한 값이 k 보다 작다면 시작 포인터를 오른쪽으로 이동
4. 계산한 값이 k 보다 크다면 끝 포인터를 왼쪽으로 이동

```js
const solution = (m, arr) => {
  let answer = [];
  let p1 = 0;
  let p2 = arr.length - 1;

  arr.sort((a, b) => a - b);
  while (p1 < p2) {
    const sum = arr[p1] + arr[p2];
    if (sum < m) p1++;
    if (sum > m) p2--;
    else {
      answer.push([arr[p1], arr[p2]]);
      p1++;
      p2--;
    }
  }
  return answer;
};

let a = [1, 2, 5, 8, 3, 4, 6];
console.log(solution(6, a));
```

### Sliding Window Algorithm;슬라이딩 윈도우 알고리즘

- 배열이나 문자열에서 *고정 크기의 윈도우(범위)*를 사용하여 배열 왼쪽에서 오른쪽으로 이동하는 알고리즘
- 교집합의 정보를 가지면서 양쪽 끝 요소만 갱신
- k 의 크기를 가진 연속부분수열의 합계나 평균을 구할 때

1. 필요 시 배열 정렬
2. 0 번째부터 k - 1 번째 요소까지 모두 계산
3. 시작 포인터는 0 번째, 끝 포인터는 k 번째로 초기화
4. 새로운 값(끝 포인터)과 가장 오래된 값(시작 포인터) 처리
5. 시작 포인터와 끝 포인터 모두 오른쪽으로 순차 이동

```js
// section5 5번 문제
const solution = (k, arr) => {
  let answer,
    sum = 0;

  for (let i = 0; i < k; i++) sum += arr[i];
  answer = sum;
  for (let i = k; i < arr.length; i++) {
    // p1 = i - k, p2 = i
    // 새로운 값 arr[i] 를 더하고, 가장 오래된 값 arr[i - k] 를 뺀다.
    // 12 + 15 + 11 에서, + 20(arr[3]) - 12(arr[0])
    sum += arr[i] - arr[i - k];
    answer = Math.max(answer, sum);
  }
  return answer;
};

let a = [12, 15, 11, 20, 25, 10, 20, 19, 13, 15];
console.log(solution(3, a));
```
