# 치즈(Gold 4)

### 문제

N×M (5≤N, M≤100)의 모눈종이 위에 아주 얇은 치즈가 <그림 1>과 같이 표시되어 있다. 단, N 은 세로 격자의 수이고, M 은 가로 격자의 수이다. 이 치즈는 냉동 보관을 해야만 하는데 실내온도에 내어놓으면 공기와 접촉하여 천천히 녹는다. 그런데 이러한 모눈종이 모양의 치즈에서 각 치즈 격자(작 은 정사각형 모양)의 4변 중에서 적어도 2변 이상이 실내온도의 공기와 접촉한 것은 정확히 한시간만에 녹아 없어져 버린다. 따라서 아래 <그림 1> 모양과 같은 치즈(회색으로 표시된 부분)라면 C로 표시된 모든 치즈 격자는 한 시간 후에 사라진다.   

<p align="center">
    <img src="https://upload.acmicpc.net/a4998beb-104c-4e37-b3d7-fd91cd81464a/-/preview/" width=250>
</p>

<p align="center">
    <그림 1>
</p>

<그림 2>와 같이 치즈 내부에 있는 공간은 치즈 외부 공기와 접촉하지 않는 것으로 가정한다. 그러므 로 이 공간에 접촉한 치즈 격자는 녹지 않고 C로 표시된 치즈 격자만 사라진다. 그러나 한 시간 후, 이 공간으로 외부공기가 유입되면 <그림 3>에서와 같이 C로 표시된 치즈 격자들이 사라지게 된다.   
    
<p align="center">
    <img src="https://upload.acmicpc.net/e5d519ee-53ea-40a6-b970-710cca0db128/-/preview/" width=250>
</p>

<p align="center">
    <그림 2>
</p>

<p align="center">
    <img src="https://upload.acmicpc.net/a00b876a-86dc-4a82-a030-603a9b1593cc/-/preview/" width=250>
</p>

<p align="center">
    <그림 3>
</p>

모눈종이의 맨 가장자리에는 치즈가 놓이지 않는 것으로 가정한다. 입력으로 주어진 치즈가 모두 녹아 없어지는데 걸리는 정확한 시간을 구하는 프로그램을 작성하시오.   

---

#### 입력
    
첫째 줄에는 모눈종이의 크기를 나타내는 두 개의 정수 N, M (5 ≤ N, M ≤ 100)이 주어진다. 그 다음 N개의 줄에는 모눈종이 위의 격자에 치즈가 있는 부분은 1로 표시되고, 치즈가 없는 부분은 0으로 표시된다. 또한, 각 0과 1은 하나의 공백으로 분리되어 있다.

---

#### 출력

출력으로는 주어진 치즈가 모두 녹아 없어지는데 걸리는 정확한 시간을 정수로 첫 줄에 출력한다.

---

#### 예제 입력 1
~~~
8 9
0 0 0 0 0 0 0 0 0
0 0 0 1 1 0 0 0 0
0 0 0 1 1 0 1 1 0
0 0 1 1 1 1 1 1 0
0 0 1 1 1 1 1 0 0
0 0 1 1 0 1 1 0 0
0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0
~~~

#### 예제 출력 1
~~~
4
~~~

[출처](https://www.acmicpc.net/problem/2638)

---

### 문제풀이

이번 문제는 bfs 문제입니다.   

이 문제를 풀 때, 어려웠던 점은 문제를 이해하는 것이었습니다.   
    
처음에 문제를 풀 때, 모눈종이의 가장자리가 의미하는 것이 주어진 좌표 바깥쪽이라고 잘못 생각해서 문제를 어렵게 풀었습니다.   
(어렵게 푼 방법으로도 정답을 맞추긴 했습니다.)

그런데 다른 분들의 풀이를 보니 제가 가장자리의 의미를 잘못 알고 있다는 것을 알았습니다.   
    
왠지 난이도에 비해 너무 어렵다고 생각했는데, 제 실수였습니다.

그래서 문제를 다시 풀어서 제출했습니다.

이 문제의 풀이방법은 다음과 같습니다.

1. (0, 0) 좌표에서 bfs를 실행한다.

2. 가장자리에는 치즈가 놓이지 않으므로 bfs를 실행하면 치즈 외부의 공기를 알아낼 수 있다.

3. 치즈 외부 공기에 2회 이상 노출된 치즈를 지운다.

4. 모든 치즈가 지워질 때까지 반복한다.
    
저는 bfs를 실행하고, 지우는 부분이 분리돼 있는데, 이걸 한 번에 하신 분이 있어서 아래에 코드를 첨부합니다.

---

#### 나의 풀이


1. 잘못 파악한 문제 풀이
~~~python
# 가장자리를 좌표 바로 바깥으로 착각한 풀이
from collections import deque

n, m = map(int, input().split())
board = [list(map(int, input().split())) for _ in range(n)]

# 좌표에 바깥에 도달한 공기들은 치즈 내부의 공기가 아님
# 도달하지 못한 공기들은 치즈 내부의 공기임
# 치즈 내부 공기를 탐색하는 함수
def bfs(row, col):
    queue = deque([(row, col)])
    visited[row][col] = True
    air_in_cheese = [(row, col)]
    no_air = False

    while queue:
        x, y = queue.popleft()

        for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
            nx, ny = x + dx, y + dy

            if not (0 <= nx < n and 0 <= ny < m):
                no_air = True
                continue

            if not visited[nx][ny] and not board[nx][ny]:
                visited[nx][ny] = True
                queue.append((nx, ny))
                air_in_cheese.append((nx, ny))

    if no_air:
        return False
    
    for x, y in air_in_cheese:
        cheese_area[x][y] = True

    return True

# 외부 공기에 2변이상 노출된 치즈를 지우는 함수
def remove_cheese():
    remove = []
    for row in range(n):
        for col in range(m):
            air_cnt = 0
            if board[row][col]:
                for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                    nx, ny = row + dx, col + dy
                    if (not (0 <= nx < n and 0 <= ny < m)) or (not board[nx][ny] and not cheese_area[nx][ny]):
                        air_cnt += 1
                if air_cnt > 1:
                    remove.append((row, col))

    for x, y in remove:
        board[x][y] = 0

    return True if remove else False


answer = 0

# 모든 치즈가 지워질 때까지 
while True:
    visited = [[False] * m for _ in range(n)]
    cheese_area = [v[:] for v in visited]

    for i in range(n):
        for j in range(m):
            if not board[i][j] and not visited[i][j]:
                bfs(i, j)

    if not remove_cheese():
        break

    answer += 1

print(answer)
~~~

2. 다시 제출한 풀이
~~~python
from collections import deque
n, m = map(int, input().split())
board = [list(map(int, input().split())) for _ in range(n)]
# (0, 0)에서 bfs를 실행해 외부 공기 부분만 방문처리
def bfs():
    queue = deque([(0, 0)])
    while queue:
        x, y = queue.popleft()

        for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
            nx, ny = x + dx, y + dy

            if 0 <= nx < n and 0 <= ny < m and not visited[nx][ny] and not board[nx][ny]:
                visited[nx][ny] = True
                queue.append((nx, ny))
# 방문 처리된 외부 공기를 2변 이상 만난 치즈들을 지움
def remove_cheese():
    remove = []
    for x in range(n):
        for y in range(m):
            if board[x][y]:
                air_cnt = 0
                for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                    nx, ny = x + dx, y + dy

                    if 0 <= nx < n and 0 <= ny < m and visited[nx][ny]:
                        air_cnt += 1
                if air_cnt >= 2:
                    remove.append((x, y))

    for x, y in remove:
        board[x][y] = 0

    return remove

answer = 0
# 모든 치즈가 지워질 때까지 반복
while True:
    visited = [[False] * m for i in range(n)]
    bfs()

    if not remove_cheese():
        break
        
    answer += 1

print(answer)
~~~

---

#### 다른 사람의 풀이

~~~python
import sys
input = sys.stdin.readline
from collections import deque

dx = [1, -1, 0, 0]
dy = [0, 0, -1, 1]

def bfs():
    visited = [[0] * m for _ in range(n)]
    q = deque()
    q.append([0, 0])
    visited[0][0] = 1
    melt = []

    while q:
    x, y = q.popleft()
        for i in range(4):
            nx, ny = x + dx[i], y + dy[i]
            if 0 <= nx < n and 0 <= ny < m:
                if cheese[nx][ny] == 0:
                    if not visited[nx][ny]:
                        q.append([nx, ny])
                        visited[nx][ny] = 1
                else:
                    visited[nx][ny] += 1
                    if visited[nx][ny] == 2:
                        melt.append([nx, ny])
		
    for x, y in melt:
        cheese[x][y] = 0

n, m = map(int, input().split())
cheese = [list(map(int, input().split())) for _ in range(n)]
t = 0
while True:
    tmp = sum([sum(i) for i in cheese])
    if tmp == 0:
        print(t)
        break

    bfs()
    t += 1
~~~
