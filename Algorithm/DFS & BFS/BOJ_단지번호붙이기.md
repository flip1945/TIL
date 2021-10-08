# 단지번호붙이기 (Silver 1)

### 문제

<그림 1>과 같이 정사각형 모양의 지도가 있다. 1은 집이 있는 곳을, 0은 집이 없는 곳을 나타낸다. 철수는 이 지도를 가지고 연결된 집의 모임인 단지를 정의하고, 단지에 번호를 붙이려 한다. 여기서 연결되었다는 것은 어떤 집이 좌우, 혹은 아래위로 다른 집이 있는 경우를 말한다. 대각선상에 집이 있는 경우는 연결된 것이 아니다. <그림 2>는 <그림 1>을 단지별로 번호를 붙인 것이다. 지도를 입력하여 단지수를 출력하고, 각 단지에 속하는 집의 수를 오름차순으로 정렬하여 출력하는 프로그램을 작성하시오.   

<p align="center">
    <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/ITVH9w1Gf6eCRdThfkegBUSOKd.png">
</p>

---

#### 입력

첫 번째 줄에는 지도의 크기 N(정사각형이므로 가로와 세로의 크기는 같으며 5≤N≤25)이 입력되고, 그 다음 N줄에는 각각 N개의 자료(0혹은 1)가 입력된다.

---

#### 출력

첫 번째 줄에는 총 단지수를 출력하시오. 그리고 각 단지내 집의 수를 오름차순으로 정렬하여 한 줄에 하나씩 출력하시오.

---

#### 예제 입력 1
~~~
7
0110100
0110101
1110101
0000111
0100000
0111110
0111000
~~~

#### 예제 출력 1
~~~
3
7
8
9
~~~

출처 : https://www.acmicpc.net/problem/2667

---

### 문제풀이

이번 문제는 bfs 혹은 dfs로 풀 수 있는 문제입니다.   

모든 영역의 크기를 구하는 문제입니다.   

dfs나 bfs를 실행하면서 상하좌우로 붙어 있는 영역의 크기를 모두 구하면 됩니다.   

저는 bfs와 dfs로 사용해 문제를 2번 풀었습니다.   

---

#### 나의 풀이

1. bfs로 푼 풀이
~~~python
from collections import deque

n = int(input())
city = [list(map(int, input())) for _ in range(n)]
answer = []

def bfs(row, col):
    result = 0
    way = [(0, -1), (0, 1), (-1, 0), (1, 0)]
    queue = deque([(row, col)])

    while queue:
        x, y = queue.popleft()

        if not city[x][y]:
            continue

        city[x][y] = 0
        result += 1

        for dx, dy in way:
            dx, dy = dx + x, dy + y

            if 0 <= dx < n and 0 <= dy < n and city[dx][dy]:
                queue.append((dx, dy))

    return result

for row in range(n):
    for col in range(n):
        if city[row][col]:
            answer.append(bfs(row, col))

print(len(answer))
print(*sorted(answer), sep='\n')
~~~

2. dfs로 푼 풀이
~~~python
n = int(input())
board = [list(map(int, input())) for _ in range(n)]
answer = []

def dfs(x, y):
    global result

    board[x][y] = 0
    result += 1

    for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
        nx, ny = x + dx, y + dy

        if 0 <= nx < n and 0 <= ny < n and board[nx][ny]:
            dfs(nx, ny)

for row in range(n):
    for col in range(n):
        if board[row][col]:
            result = 0
            dfs(row, col)
            answer.append(result)

print(len(answer))
print(*sorted(answer), sep='\n')
~~~

