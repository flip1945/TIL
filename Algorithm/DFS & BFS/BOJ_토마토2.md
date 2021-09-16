# 토마토2 (Silver 1)

### 문제

철수의 토마토 농장에서는 토마토를 보관하는 큰 창고를 가지고 있다. 토마토는 아래의 그림과 같이 격자모양 상자의 칸에 하나씩 넣은 다음, 상자들을 수직으로 쌓아 올려서 창고에 보관한다.   

<p align="center">
  <img src="https://upload.acmicpc.net/c3f3343d-c291-40a9-9fe3-59f792a8cae9/-/preview/" width=215>
</p>

창고에 보관되는 토마토들 중에는 잘 익은 것도 있지만, 아직 익지 않은 토마토들도 있을 수 있다. 보관 후 하루가 지나면, 익은 토마토들의 인접한 곳에 있는 익지 않은 토마토들은 익은 토마토의 영향을 받아 익게 된다. 하나의 토마토에 인접한 곳은 위, 아래, 왼쪽, 오른쪽, 앞, 뒤 여섯 방향에 있는 토마토를 의미한다. 대각선 방향에 있는 토마토들에게는 영향을 주지 못하며, 토마토가 혼자 저절로 익는 경우는 없다고 가정한다. 철수는 창고에 보관된 토마토들이 며칠이 지나면 다 익게 되는지 그 최소 일수를 알고 싶어 한다.   

토마토를 창고에 보관하는 격자모양의 상자들의 크기와 익은 토마토들과 익지 않은 토마토들의 정보가 주어졌을 때, 며칠이 지나면 토마토들이 모두 익는지, 그 최소 일수를 구하는 프로그램을 작성하라. 단, 상자의 일부 칸에는 토마토가 들어있지 않을 수도 있다.

---

#### 입력

첫 줄에는 상자의 크기를 나타내는 두 정수 M,N과 쌓아올려지는 상자의 수를 나타내는 H가 주어진다. M은 상자의 가로 칸의 수, N은 상자의 세로 칸의 수를 나타낸다. 단, 2 ≤ M ≤ 100, 2 ≤ N ≤ 100, 1 ≤ H ≤ 100 이다. 둘째 줄부터는 가장 밑의 상자부터 가장 위의 상자까지에 저장된 토마토들의 정보가 주어진다. 즉, 둘째 줄부터 N개의 줄에는 하나의 상자에 담긴 토마토의 정보가 주어진다. 각 줄에는 상자 가로줄에 들어있는 토마토들의 상태가 M개의 정수로 주어진다. 정수 1은 익은 토마토, 정수 0 은 익지 않은 토마토, 정수 -1은 토마토가 들어있지 않은 칸을 나타낸다. 이러한 N개의 줄이 H번 반복하여 주어진다.   

토마토가 하나 이상 있는 경우만 입력으로 주어진다.

---

#### 출력

여러분은 토마토가 모두 익을 때까지 최소 며칠이 걸리는지를 계산해서 출력해야 한다. 만약, 저장될 때부터 모든 토마토가 익어있는 상태이면 0을 출력해야 하고, 토마토가 모두 익지는 못하는 상황이면 -1을 출력해야 한다.

---

#### 예제 입력 1
~~~
5 3 1
0 -1 0 0 0
-1 -1 0 1 1
0 0 0 1 1
~~~

#### 예제 출력 1
~~~
-1
~~~

#### 예제 입력 2
~~~
5 3 2
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 1 0 0
0 0 0 0 0
~~~

#### 예제 출력 2
~~~
4
~~~

#### 예제 입력 3
~~~
4 3 2
1 1 1 1
1 1 1 1
1 1 1 1
1 1 1 1
-1 -1 -1 -1
1 1 1 -1
~~~

#### 예제 출력 3
~~~
0
~~~

출처 : https://www.acmicpc.net/problem/7576

---

### 문제풀이

이번 문제는 bfs로 풀 수 있는 문제입니다.   

다른 문제들과의 차이점은 이 문제는 3차원 배열에 저장된 요소에서 bfs를 실행해야 한다는 점입니다.   

x, y 좌표에 높이를 나타내는 z가 추가된 형태에서 bfs를 실행하면 정답을 맞출 수 있습니다.

---

#### 나의 풀이

~~~python
from collections import deque

m, n, h = map(int, input().split())
tomatoes = [[list(map(int, input().split())) for _ in range(n)] for _ in range(h)]
queue = deque()
# queue에 토마토 입력
for i in range(h):
    for j in range(n):
        for k in range(m):
            if tomatoes[i][j][k] == 1:
                queue.append((i, j, k))
# bfs 실행
answer = 1
while queue:
    height, row, col = queue.popleft()

    for dh, dx, dy in [(0, -1, 0), (0, 1, 0), (0, 0, -1), (0, 0, 1), (-1, 0, 0), (1, 0, 0)]:
        nh, nx, ny = height + dh, row + dx, col + dy

        if 0 <= nh < h and 0 <= nx < n and 0 <= ny < m and not tomatoes[nh][nx][ny]:
            tomatoes[nh][nx][ny] = tomatoes[height][row][col] + 1
            queue.append((nh, nx, ny))
            answer = max(answer, tomatoes[nh][nx][ny])
# 익지 않은 토마토가 있다면 -1을 출력하고 
for i in range(h):
    for j in range(n):
        for k in range(m):
            if not tomatoes[i][j][k]:
                print(-1)
                exit()

print(answer - 1)
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/13490502

~~~python
import sys
from collections import deque

inp = sys.stdin.readline

M,N,H = map(int,inp().split())
# M가로, N세로, H높이

level = 0
box = list()
queue = deque()

box = [[list(map(int,inp().split())) for _ in range(N) ]for _ in range(H)]
queue = deque((level,z,y,x) for z in range(H) for y in range(N) for x in range(M) if box[z][y][x]==1)

while queue :
    level,z,y,x = queue.popleft()

    for h,n,m in (1,0,0),(-1,0,0),(0,1,0),(0,-1,0),(0,0,1),(0,0,-1) :
        if 0 <=z+h< H and 0 <=y+n< N and 0 <=x+m< M :
            if not box[z+h][y+n][x+m] :
                box[z+h][y+n][x+m] = 1
                queue.append((level+1,z+h,y+n,x+m))

for z in range(H) :
    for y in range(N) :
        for x in range(M) :
            if box[z][y][x] == 0 : level=-1;break

print(level)
~~~
