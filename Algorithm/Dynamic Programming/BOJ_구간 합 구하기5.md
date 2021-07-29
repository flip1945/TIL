# 구간 합 구하기5 (Silver 1)

### 문제

N×N개의 수가 N×N 크기의 표에 채워져 있다. (x1, y1)부터 (x2, y2)까지 합을 구하는 프로그램을 작성하시오. (x, y)는 x행 y열을 의미한다.   

예를 들어, N = 4이고, 표가 아래와 같이 채워져 있는 경우를 살펴보자.   

~~~
1	2	3	4
2	3	4	5
3	4	5	6
4	5	6	7
~~~

여기서 (2, 2)부터 (3, 4)까지 합을 구하면 3+4+5+4+5+6 = 27이고, (4, 4)부터 (4, 4)까지 합을 구하면 7이다.   

표에 채워져 있는 수와 합을 구하는 연산이 주어졌을 때, 이를 처리하는 프로그램을 작성하시오.   

---

#### 입력

첫째 줄에 표의 크기 N과 합을 구해야 하는 횟수 M이 주어진다. (1 ≤ N ≤ 1024, 1 ≤ M ≤ 100,000) 둘째 줄부터 N개의 줄에는 표에 채워져 있는 수가 1행부터 차례대로 주어진다. 다음 M개의 줄에는 네 개의 정수 x1, y1, x2, y2 가 주어지며, (x1, y1)부터 (x2, y2)의 합을 구해 출력해야 한다. 표에 채워져 있는 수는 1,000보다 작거나 같은 자연수이다. (x1 ≤ x2, y1 ≤ y2)   

---

#### 출력

총 M줄에 걸쳐 (x1, y1)부터 (x2, y2)까지 합을 구해 출력한다.

---

#### 예제 입력 1
~~~
4 3
1 2 3 4
2 3 4 5
3 4 5 6
4 5 6 7
2 2 3 4
3 4 3 4
1 1 4 4
~~~

#### 예제 출력 1
~~~
27
6
64
~~~

#### 예제 입력 2
~~~
2 4
1 2
3 4
1 1 1 1
1 2 1 2
2 1 2 1
2 2 2 2
~~~

#### 예제 출력 2
~~~
1
2
3
4
~~~

[출처](https://www.acmicpc.net/problem/11660)

---

### 문제풀이

이번 문제는 prefix sum + Dynamic Programming 문제입니다.   

입력의 숫자가 많기 때문에 일반적인 반복문을 이용해 풀려고 하면 시간 초과 판정을 받습니다.   

그래서 다이나믹 프로그래밍 알고리즘을 사용해 각 위치의 구간 합을 구해줘야 합니다.   

각 위치의 구간 합은 (1,1)에서 그 위치까지의 모든 숫자를 더한 값입니다.   

예를 들어 (4,4)의 구간합은 (1,1)~(4,4)에 위치한 모든 숫자를 더한 값이 되는 것입니다.   

이런 방식으로 구간 합을 구하면 x1, y1, x2, y2가 주어졌을 때, 두 지점 사이의 구간합을 구할 수 있습니다.   

(x1, y1)~(x2, y2)에 위치한 모든 숫자를 더한 값을 구하는 방법은 (1,1)~(x2,y2)의 값을 구하고, 거기에서 (1,1)~(x2,y1-1), (1,1)~(x1-1,y2) 값을 모두 빼줍니다.   

이렇게 하면 (1,1)~(x1-1,y1-1) 부분이 겹쳐서 빠지는 문제가 생기는 데, 여기에 다시 (1,1)~(x1-1,y1-1) 값을 더해주면 정답을 맞출 수 있습니다.   

---

#### 나의 풀이

~~~python
import sys
n, m = map(int, input().split())
board = [list(map(int, sys.stdin.readline().split())) for _ in range(n)]
dp = [[0]*(n+1) for _ in range(n+1)]

for row in range(n):
    for col in range(n):
        dp[row+1][col+1] = dp[row][col+1] + dp[row+1][col] - dp[row][col] + board[row][col]

for i in range(m):
    x1, y1, x2, y2 = map(int, sys.stdin.readline().split())
    print(dp[x2][y2] - dp[x2][y1-1] - dp[x1-1][y2] + dp[x1-1][y1-1])
~~~

---

#### 다른 사람의 풀이

출처 : https://velog.io/@ready2start/Python-%EB%B0%B1%EC%A4%80-11660-%EA%B5%AC%EA%B0%84-%ED%95%A9-%EA%B5%AC%ED%95%98%EA%B8%B0-5

~~~python
from sys import stdin


n, m = map(int, stdin.readline().split())

numbers = [[0] * (n + 1)]

for _ in range(n):
    nums = [0] + [int(x) for x in stdin.readline().split()]
    numbers.append(nums)

# prefix sum 행렬 만들기

# 1. 행 별로 더하기
for i in range(1, n + 1):
    for j in range(1, n):
        numbers[i][j + 1] += numbers[i][j]

# 2. 열 별로 더하기
for j in range(1, n + 1):
    for i in range(1, n):
        numbers[i + 1][j] += numbers[i][j]

for _ in range(m):
    x1, y1, x2, y2 = map(int, stdin.readline().split())
    # (x1, y1)에서 (x2, y2)까지의 합
    # p[x2][y2] - p[x1 - 1][y2] - p[x2][y1 - 1] + p[x1 - 1][y1 - 1]
    print(numbers[x2][y2] - numbers[x1 - 1][y2] - numbers[x2][y1 - 1] + numbers[x1 - 1][y1 - 1])
~~~
