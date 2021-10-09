# 아기 상어2 (Silver 2)

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

출처 : https://www.acmicpc.net/problem/17086

---

### 문제풀이

이번 문제는 상어의 위치를 파악하는 탐색 문제입니다.   

최단 거리를 알아내야 하기 때문에 bfs로 문제를 해결할 수 있습니다.   

좌표의 최대 값이 50 * 50이라서 풀면 바로 풀릴 줄 알았는데, 제 착각이었습니다.   

방문 리스트 없이 문제를 풀려고 하니 시간 초과 판정을 받았습니다.   
그래서 바로 방문 리스트를 만들었고, 그제서야 통과할 수 있었습니다.   

그런데 문제를 풀고나니 결린 시간이 너무 컸습니다.   

다른 사람들은 문제를 어떻게 풀었나 봤는데, 좋은 풀이 방법이 많았습니다.   

1. 저는 상어의 위치를 시작점으로 잡나 사람의 위치를 시작점으로 잡나 같다고 생각했습니다.
2. 두 가지 방법 모두 매 좌표에서 모든 좌표를 훑어야 한다고 생각했습니다.
3. 그런데 처음에 상어의 위치를 모두 queue에 넣고, bfs를 실행하면 O(n)시간에 문제를 풀 수 있습니다.

bfs를 하기 전에 특정 좌표를 모두 queue에 넣는다는 발상은 못 해봤는데, 또 하나 알게돼서 재밌었습니다.

---

#### 나의 풀이

~~~python
from collections import deque

n, m = map(int, input().split())
answer = []

graph = [list(map(int, input().split())) for i in range(n)]
# bfs 함수
def bfs(row, col):
    # queue에 시작점의 좌표, 상어까지의 최단 거리를 push
    que = deque([(row, col, 0)])
    # 8가지 방향으로 탐색하기 위한 방향 리스트
    way = [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]
    # 방문 리스트 초기화
    visited = [[False] * m for _ in range(n)]
    # 시작점의 좌표는 방문처리
    visited[row][col] = True
    # bfs 실행
    while que:
        x, y, sec = que.popleft()
        # 현재 좌표에 상어가 있다면 지금까지의 시간을 반환하고 종료
        if graph[x][y]:
            return sec
        # 8가지 방향으로 탐색
        for dx, dy in way:
            dx += x
            dy += y
            # 좌표를 벗어나지 않고, 방문하지 않은 경우만 실행
            if 0 <= dx < n and 0 <= dy < m and not visited[dx][dy]:
                # 다음에 갈 좌표를 방문처리
                visited[dx][dy] = True
                que.append((dx, dy, sec + 1))
    # 상어가 1마리도 없는 경우 예외처리
    return 0
# 모든 좌표에서 상어까지의 최단 거리를 탐색
for row in range(n):
    for col in range(m):
        answer.append(bfs(row, col))

print(max(answer))
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/12489529

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
