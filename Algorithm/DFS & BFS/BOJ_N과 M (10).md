# N과 M (10) (Silver 2)

### 문제

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

* N개의 자연수 중에서 M개를 고른 수열
* 고른 수열은 비내림차순이어야 한다.
  * 길이가 K인 수열 A가 A1 ≤ A2 ≤ ... ≤ AK-1 ≤ AK를 만족하면, 비내림차순이라고 한다.

---

#### 입력

첫째 줄에 N과 M이 주어진다. (1 ≤ M ≤ N ≤ 8)

둘째 줄에 N개의 수가 주어진다. 입력으로 주어지는 수는 10,000보다 작거나 같은 자연수이다.

---

#### 출력

한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력한다. 중복되는 수열을 여러 번 출력하면 안되며, 각 수열은 공백으로 구분해서 출력해야 한다.

수열은 사전 순으로 증가하는 순서로 출력해야 한다.

출처 : https://www.acmicpc.net/problem/15664

---

### 문제풀이

이 문제는 백트래킹 문제입니다.

비내림차순 수열을 만들기 위해 처음에 입력받은 숫자들을 정렬한 뒤에 백트래킹을 진행했습니다.

그리고 중복된 순열을 제거하기 위해서 set을 이용해 중복된 순열을 제거해줬습니다.

그 외에는 평범한 백트래킹을 이용해 풀이를 하면 됩니다.

---

#### 나의 풀이

~~~python
n, m = map(int, input().split())
a = sorted(list(map(int, input().split())))

def dfs(permutations, result, current_index, current_depth):
    if current_depth == m:
        result.add(tuple(permutations))
        return
    for i in range(current_index, n):
        dfs(permutations + [a[i]], result, i + 1, current_depth + 1)
    return result

for s in sorted(dfs([], set(), 0, 0)):
    print(*s)
~~~
