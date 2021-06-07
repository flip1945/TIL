# 아기 상어 2(Silver 2)

### 문제

N×M 크기의 공간에 아기 상어 여러 마리가 있다. 공간은 1×1 크기의 정사각형 칸으로 나누어져 있다. 한 칸에는 아기 상어가 최대 1마리 존재한다.   

어떤 칸의 안전 거리는 그 칸과 가장 거리가 가까운 아기 상어와의 거리이다. 두 칸의 거리는 하나의 칸에서 다른 칸으로 가기 위해서 지나야 하는 칸의 수이고, 이동은 인접한 8방향(대각선 포함)이 가능하다.   

안전 거리가 가장 큰 칸을 구해보자.   

---

#### 입력

첫째 줄에 공간의 크기 N과 M(2 ≤ N, M ≤ 50)이 주어진다. 둘째 줄부터 N개의 줄에 공간의 상태가 주어지며, 0은 빈 칸, 1은 아기 상어가 있는 칸이다. 빈 칸의 개수가 한 개 이상인 입력만 주어진다.   

---

#### 출력

첫째 줄에 안전 거리의 최댓값을 출력한다.

---

#### 예제 입력 1
~~~
5 4
0 0 1 0
0 0 0 0
1 0 0 0
0 0 0 0
0 0 0 1
~~~

#### 예제 출력 1
~~~
2
~~~

#### 예제 입력 2
~~~
7 4
0 0 0 1
0 1 0 0
0 0 0 0
0 0 0 1
0 0 0 0
0 1 0 0
0 0 0 1
~~~

#### 예제 출력 2
~~~
2
~~~

[출처](https://www.acmicpc.net/problem/17086)

---

### 문제풀이

  

---

#### 나의 풀이

~~~python
from collections import deque

n, m = map(int, input().split())
answer = []

graph = [list(map(int, input().split())) for i in range(n)]

def bfs(row, col):
    que = deque([(row, col, 0)])
    way = [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]
    visited = [[False] * m for _ in range(n)]
    visited[row][col] = True

    while que:
        x, y, sec = que.popleft()

        if graph[x][y]:
            return sec

        for dx, dy in way:
            dx += x
            dy += y

            if 0 <= dx < n and 0 <= dy < m and not visited[dx][dy]:
                visited[dx][dy] = True
                que.append((dx, dy, sec + 1))
    return 0

for row in range(n):
    for col in range(m):
        answer.append(bfs(row, col))

print(max(answer))
~~~

---

#### 다른 사람의 풀이

~~~python
from collections import deque

n, m = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(n)]
dx = (-1, -1, -1, 0, 0, 1, 1, 1)
dy = (-1, 0, 1, -1, 1, -1, 0, 1)
q = deque()

def bfs():
    while q:
        x, y = q.popleft()
        for k in range(8):
            nx, ny = x+dx[k], y+dy[k]
            if nx < 0 or nx >= n or ny < 0 or ny >= m:
                continue
            if not a[nx][ny]:
                q.append((nx, ny))
                a[nx][ny] = a[x][y]+1

for i in range(n):
    for j in range(m):
        if a[i][j]:
            q.append((i, j))
bfs()
print(max(map(max, a))-1)
~~~
