# 욕심쟁이 판다 (Gold 3)

### 문제

n × n의 크기의 대나무 숲이 있다. 욕심쟁이 판다는 어떤 지역에서 대나무를 먹기 시작한다. 그리고 그 곳의 대나무를 다 먹어 치우면 상, 하, 좌, 우 중 한 곳으로 이동을 한다. 그리고 또 그곳에서 대나무를 먹는다. 그런데 단 조건이 있다. 이 판다는 매우 욕심이 많아서 대나무를 먹고 자리를 옮기면 그 옮긴 지역에 그 전 지역보다 대나무가 많이 있어야 한다.   

이 판다의 사육사는 이런 판다를 대나무 숲에 풀어 놓아야 하는데, 어떤 지점에 처음에 풀어 놓아야 하고, 어떤 곳으로 이동을 시켜야 판다가 최대한 많은 칸을 방문할 수 있는지 고민에 빠져 있다. 우리의 임무는 이 사육사를 도와주는 것이다. n × n 크기의 대나무 숲이 주어져 있을 때, 이 판다가 최대한 많은 칸을 이동하려면 어떤 경로를 통하여 움직여야 하는지 구하여라.

---

#### 입력

첫째 줄에 대나무 숲의 크기 n(1 ≤ n ≤ 500)이 주어진다. 그리고 둘째 줄부터 n+1번째 줄까지 대나무 숲의 정보가 주어진다. 대나무 숲의 정보는 공백을 사이로 두고 각 지역의 대나무의 양이 정수 값으로 주어진다. 대나무의 양은 1,000,000보다 작거나 같은 자연수이다.

---

#### 출력

첫째 줄에는 판다가 이동할 수 있는 칸의 수의 최댓값을 출력한다.

---

#### 예제 입력 1
~~~
4
14 9 12 10
1 11 5 4
7 15 2 13
6 3 16 8
~~~

#### 예제 출력 1
~~~
4
~~~

[출처](https://www.acmicpc.net/problem/1937)

---

### 문제풀이

이번 문제는 Dynamic Programming + dfs 문제입니다.   

저는 Dynamic Programming 문제를 풀 때, 거의 상향식 방법으로 풀었습니다.   

그 상향식 방법만 사용한 이유는 재귀를 사용한 하향식 방법은 속도가 느리고, python에서는 별도로 최대 재귀 재한을 풀어줘야 하는 번거로움이 있기 때문입니다.

그런데 이번 문제는 도저히 상향식 풀이가 떠오르지 않아 하향식 풀이로 풀게 됐습니다.

하향식 방법으로 문제를 푼 결과 생각보다 쉽게 풀려서 놀랐고, 앞으로는 하향식 풀이 방법도 염두하면서 풀이해야 겠다는 생각이 들었습니다.   

다음은 문제를 해결한 방법입니다.

1. 2차원으로 구성된 dp 테이블을 설정합니다.
2. 모든 행렬에서 dfs 방식으로 판다가 이동할 수 있는 칸을 탐색합니다.
3. 탐색을 하면서 dp 테이블을 갱신합니다.
4. 갱신된 dp 테이블을 탐색하는 경우, 이미 최대 값으로 갱신됐기 때문에 다시 탐색하지 않고 테이블에 저장된 값을 리턴 시킵니다.
5. 탐색을 종료한 후 dp 테이블에서 가장 큰 값을 정답으로 제출하면 됩니다.

---

#### 나의 풀이

1. 정답으로 제출한 풀이

~~~python
import sys
sys.setrecursionlimit(10**6)
n = int(input())
forest = [list(map(int, input().split())) for _ in range(n)]
dp = [[0] * n for _ in range(n)]

def recursive_search(x, y):
    result = 0

    dp[x][y] += 1

    for dx, dy in [(0, -1), (0, 1), (-1, 0), (1, 0)]:
        nx, ny = x + dx, y + dy

        if 0 <= nx < n and 0 <= ny < n and forest[nx][ny] < forest[x][y]:
            if dp[nx][ny]:
                result = max(result, dp[nx][ny])
            else:
                result = max(result, recursive_search(nx, ny))

    dp[x][y] += result

    return dp[x][y]


for i in range(n):
    for j in range(n):
        if not dp[i][j]:
            recursive_search(i, j)

print(max(map(max, dp)))
~~~

2. 제출 후 다듬은 풀이

~~~python
import sys
sys.setrecursionlimit(10**6)
n = int(input())
forest = [list(map(int, input().split())) for _ in range(n)]
dp = [[0] * n for _ in range(n)]

def recursive_search(x, y):
    if dp[x][y]:
        return dp[x][y]

    result = 0

    for dx, dy in [(0, -1), (0, 1), (-1, 0), (1, 0)]:
        nx, ny = x + dx, y + dy

        if 0 <= nx < n and 0 <= ny < n and forest[nx][ny] < forest[x][y]:
            result = max(result, recursive_search(nx, ny))

    dp[x][y] += result + 1

    return dp[x][y]

for i in range(n):
    for j in range(n):
        recursive_search(i, j)

print(max(map(max, dp)))
~~~

---

#### 다른 사람의 풀이

~~~pyhton
n = int(input())
forest = [list(map(int,input().split())) for i in range(n)]
best = [[-1]*n for i in range(n)]
pos = [(forest[i][j],i,j) for i in range(n) for j in range(n)]
pos.sort(reverse=True)

def get(L,i,j):
    if 0<=i<n and 0<=j<n:
        return L[i][j]
    return -1

for f,i,j in pos:
    prev = 0
    for newi, newj in ((i-1,j), (i+1,j), (i,j-1), (i,j+1)):
        if get(forest,newi,newj) > forest[i][j]:
            prev = max(prev, best[newi][newj])
    best[i][j] = prev+1
print(max(map(max,best)))
~~~
