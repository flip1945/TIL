# 로봇 조종하기 (Gold 1)

### 문제

NASA에서는 화성 탐사를 위해 화성에 무선 조종 로봇을 보냈다. 실제 화성의 모습은 굉장히 복잡하지만, 로봇의 메모리가 얼마 안 되기 때문에 지형을 N×M 배열로 단순화 하여 생각하기로 한다.   

지형의 고저차의 특성상, 로봇은 움직일 때 배열에서 왼쪽, 오른쪽, 아래쪽으로 이동할 수 있지만, 위쪽으로는 이동할 수 없다. 또한 한 번 탐사한 지역(배열에서 하나의 칸)은 탐사하지 않기로 한다.   

각각의 지역은 탐사 가치가 있는데, 로봇을 배열의 왼쪽 위 (1, 1)에서 출발시켜 오른쪽 아래 (N, M)으로 보내려고 한다. 이때, 위의 조건을 만족하면서, 탐사한 지역들의 가치의 합이 최대가 되도록 하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 N, M(1≤N, M≤1,000)이 주어진다. 다음 N개의 줄에는 M개의 수로 배열이 주어진다. 배열의 각 수는 절댓값이 100을 넘지 않는 정수이다. 이 값은 그 지역의 가치를 나타낸다.

---

#### 출력

첫째 줄에 최대 가치의 합을 출력한다.

---

#### 예제 입력 1
~~~
5 5
10 25 7 8 13
68 24 -78 63 32
12 -69 100 -29 -25
-16 -22 -57 -33 99
7 -76 -11 77 15
~~~

#### 예제 출력 1
~~~
319
~~~

[출처](https://www.acmicpc.net/problem/2169)

---

### 문제풀이

이번 문제는 Dynamic Programming문제입니다.   

이 문제는 상향식 풀이가 쉽기 때문에 상향식으로 풀이 했습니다.   

이 문제의 요점은 로봇이 다시 위로 돌아갈 수 없다는 점과 왼쪽으로도 이동할 수 있다는 점입니다.   

위로는 돌아갈 수 없기 때문에 위의 행부터 최적 부분 구조를 만족시키면서 dp 테이블(2차원 테이블)을 채워나가면 됩니다.   

그리고 로봇이 왼쪽으로도 이동할 수 있기 때문에 오른쪽 열에서 왼쪽열로 이동하는 부분도 염두하면서 코딩해야 합니다.   

이 두 가지 방법만 주의 상당히 쉽게 풀 수 있었던 것 같습니다.

---

#### 나의 풀이

~~~python
n, m = map(int, input().split())
mars = [list(map(int, input().split())) for _ in range(n)]
dp = [[0] * m for _ in range(n)]

dp[0][0] = mars[0][0]

for col in range(1, m):
    dp[0][col] = dp[0][col-1] + mars[0][col]

for row in range(1, n):
    to_right = [0] * m
    to_left = [0] * m
    for col in range(m):
        if col == 0:
            to_right[col] = dp[row-1][col] + mars[row][col]
        else:
            to_right[col] = max(dp[row-1][col], to_right[col-1]) + mars[row][col]
    for col in range(m-1, -1, -1):
        if col == m - 1:
            to_left[col] = dp[row-1][col] + mars[row][col]
        else:
            to_left[col] = max(dp[row-1][col], to_left[col+1]) + mars[row][col]
    for col in range(m):
        dp[row][col] = max(to_right[col], to_left[col])
print(dp[n-1][m-1])
~~~

---

#### 다른 사람의 풀이

~~~python
N, M = map(int, input().split())
A = [list(map(int, input().split())) for _ in range(N)]
dp = []
S = 0
for i in range(M):
    S += A[0][i]
    dp.append(S)

for j in range(1, N):
    L = [0] * M
    L[0] = dp[0] + A[j][0]
    for i in range(1, M):
        L[i] = A[j][i] + max(L[i - 1], dp[i])
    R = [0] * M
    R[-1] = dp[-1] + A[j][-1]
    for i in range(1, M):
        R[-i-1] = A[j][-i-1] + max(R[-i], dp[-i-1])
    for i in range(M):
        dp[i] = max(L[i], R[i])

print(dp[-1])
~~~
