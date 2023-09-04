# Algorithm

- [wikidocs.net: 다빈치코딩 알고리즘](https://wikidocs.net/book/10280)

## Two Pointers Algorithm;투 포인터 알고리즘

- 슬라이딩 윈도우 알고리즘은 범위가 고정되어 있지만, 투 포인터 알고리즘은 _범위가 동적으로 변한다_.
- 이중반복문의 시간복잡도는 O(n^2) 이므로 배열 크기가 커질수록 연산량이 제곱으로 증가하는 반면,  
  투 포인터 알고리즘의 시간복잡도는 O(n) 이므로 더 효율적이다.
- 요소의 합계나 평균이 k 인 연속부분수열을 모두 찾을 때

### 배열의 처음에서 시작하여 마지막으로 이동하는 경우

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

### 배열의 양 끝에서 시작하여 가운데로 이동하는 경우

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

## Sliding Window Algorithm;슬라이딩 윈도우 알고리즘

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

## Greedy Algorithm;탐욕 알고리즘

- [geeksforgeeks: Greedy Algorithms](https://www.geeksforgeeks.org/greedy-algorithms/)
- 각 단계에서 지역 최적해를 찾음으로써 최종적으로 전역 최적해를 구하는 방법
- 완벽한 전역 최적해를 도출한다는 보장은 없으나 그 근사치를 얻을 수 있다.
- 최적화 문제를 빠르고 효율적으로 해결할 수 있다.

### 적용 조건

- 탐욕적 선택 속성 (Greedy Choice Property):  
  이전 선택이 이후의 선택에 영향을 주지 않아야 한다.  
  따라서 각 단계의 정보나 선택에 의존성이 있는 문제(재귀적 확인이 필요한 문제)에 적용될 수 없다.
- 최적 부분 구조 (Optimal Substructure):  
  문제를 부분 문제로 나누었을 때, 각 부분 문제의 최적해를 결합하면 전체 문제의 최적해를 구할 수 있어야 한다.

### 절차

1. 문제에서 주어진 리소스를 정렬
2. 최종 결과를 담을 변수 선언
3. 선택 절차(Selection Procedure): 각 단계에서 지역 최적해를 선택
4. 적절성 검사(Feasibility Check): 선택이 문제의 조건을 만족하는지 검사 후 최종 결과에 반영
5. 해답 검사(Solution Check): 전체 문제가 해결되었는지 검사 후 그렇지 않으면 선택 절차로 돌아가 과정을 반복

## Binary Search;이진탐색(이분검색)

- 정렬되어 있는 배열에서 탐색 범위를 절반으로 좁혀가면서 특정 값을 찾는 방법
- 시간복잡도는 O(logN)으로 배열 전체를 순차탐색하는 것(O(N))보다 상대적으로 빠르게 해결할 수 있다.
- 결정문제(Decision Problem)의 답이 이분적(yes/no)일 때 사용할 수 있다.

### 절차

1. 배열이 정렬되어있지 않다면 정렬
2. 배열의 처음과 끝 index를 각각 `lt`와 `rt` 변수로 정의
3. `(lt + rt) / 2` 한 중앙 index `mid` 변수 정의
   - 문제가 배열에서 값을 찾는 것이 아닐 경우, 조건 k가 있을 예상 범위에서 변수를 설정해야 한다.
4. 배열의 중앙 요소 `arr[mid]` 를 주어진 조건 k 와 비교
5. k 가 중앙 요소와 일치하면 실행문 종료, 그렇지 않을 경우 다음 탐색 범위로 사용할 방향을 선택
   - 중앙 요소가 k 보다 크면(`arr[mid] > k`) 왼쪽이 다음 탐색에 사용(`rt = mid - 1`)
   - 중앙 요소가 k 보다 작으면(`arr[mid] < k`) 오른쪽이 다음 탐색에 사용(`lt = mid + 1`)
6. k 를 찾거나(`arr[mid] === k`) 전체 탐색 범위가 소진될 때까지(`lt >= rt`) 반복

```js
// section7 10번 문제
const solution = (target, arr) => {
  let answer;
  let lt = 0;
  let rt = arr.length - 1;

  arr.sort((a, b) => a - b);
  while (lt <= rt) {
    let mid = parseInt((lt + rt) / 2);
    if (arr[mid] === target) {
      // 몇 번째에 있는지 출력하므로 + 1
      answer = mid + 1;
      break;
    }
    if (arr[mid] > target) rt = mid - 1;
    else lt = mid + 1;
  }
  return answer;
};
```

## Depth-first Search(DFS);깊이 우선 탐색

- 왼쪽 노드에서 한 방향으로 마지막 depth 까지 깊게 탐색,
  방문할 노드가 없으면 부모 노드로 거슬러 올라가 형제 노드를 탐색
- stack 을 사용하여 구현(방문할 노드 push, 방문한 노드 pop)
- 재귀적인 특성을 가짐

### 이진 트리 탐색

- 왼쪽 자식 노드: 부모 \* 2  
  오른쪽 자식 노드: 부모 \* 2 + 1
- 순회방식별 각 위치에서 부모노드를 문자열에 병합
- 재귀함수 호출 스택에 유의:  
  호출된 함수가 종료되어야 block 된 다음 코드 실행

```js
// section8 3번 문제
const solution = (n) => {
  let answer = "";
  let preOrder = ""; // 1 2 4 5 3 6 7
  let inOrder = ""; // 4 2 5 1 6 3 7
  let postOrder = ""; // 4 5 2 6 7 3 1

  const DFS = (v) => {
    if (v > 7) return;
    preOrder += v + " ";
    DFS(v * 2); // 2
    inOrder += v + " ";
    DFS(v * 2 + 1); // 3
    postOrder += v + " ";
  };

  DFS(n); // 1
  answer = `${preOrder}\n${inOrder}\n${postOrder}`;
  return answer;
};

console.log(solution(1));
```

#### 호출 순서

- DFS2 는 왼쪽 자식 노드, DFS3 은 오른쪽 자식 노드
- 모든 자식 노드의 탐색이 끝나면 부모 노드는 스택에서 pop

1. DFS1(1)
2. DFS1(1) => DFS2(2) => DFS2(4) => ~~DFS2(8)~~
3. DFS1(1) => DFS2(2) => DFS2(4) => ~~DFS3(9)~~
4. DFS1(1) => DFS2(2)
5. DFS1(1) => DFS2(2) => DFS3(5) => ~~DFS2(10)~~
6. DFS1(1) => DFS2(2) => DFS3(5) => ~~DFS3(11)~~
7. DFS1(1)
8. DFS1(1) => DFS3(3) => DFS2(6) => ~~DFS2(12)~~
9. DFS1(1) => DFS3(3) => DFS2(6) => ~~DFS3(13)~~
10. DFS1(1) => DFS3(3)
11. DFS1(1) => DFS3(3) => DFS3(7) => ~~DFS2(14)~~
12. DFS1(1) => DFS3(3) => DFS3(7) => ~~DFS2(15)~~
13. DFS1(1)
