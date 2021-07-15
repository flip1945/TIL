# 다리 만들기(Gold 3)

### 문제

여러 섬으로 이루어진 나라가 있다. 이 나라의 대통령은 섬을 잇는 다리를 만들겠다는 공약으로 인기몰이를 해 당선될 수 있었다. 하지만 막상 대통령에 취임하자, 다리를 놓는다는 것이 아깝다는 생각을 하게 되었다. 그래서 그는, 생색내는 식으로 한 섬과 다른 섬을 잇는 다리 하나만을 만들기로 하였고, 그 또한 다리를 가장 짧게 하여 돈을 아끼려 하였다.   

이 나라는 N×N크기의 이차원 평면상에 존재한다. 이 나라는 여러 섬으로 이루어져 있으며, 섬이란 동서남북으로 육지가 붙어있는 덩어리를 말한다. 다음은 세 개의 섬으로 이루어진 나라의 지도이다.   

<p align="center">
    <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/201008/bri.PNG">
</p>

위의 그림에서 색이 있는 부분이 육지이고, 색이 없는 부분이 바다이다. 이 바다에 가장 짧은 다리를 놓아 두 대륙을 연결하고자 한다. 가장 짧은 다리란, 다리가 격자에서 차지하는 칸의 수가 가장 작은 다리를 말한다. 다음 그림에서 두 대륙을 연결하는 다리를 볼 수 있다.   

<p align="center">
    <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/201008/b2.PNG">
</p>

물론 위의 방법 외에도 다리를 놓는 방법이 여러 가지 있으나, 위의 경우가 놓는 다리의 길이가 3으로 가장 짧다(물론 길이가 3인 다른 다리를 놓을 수 있는 방법도 몇 가지 있다).   

지도가 주어질 때, 가장 짧은 다리 하나를 놓아 두 대륙을 연결하는 방법을 찾으시오.   

---

#### 입력

첫 줄에는 지도의 크기 N(100이하의 자연수)가 주어진다. 그 다음 N줄에는 N개의 숫자가 빈칸을 사이에 두고 주어지며, 0은 바다, 1은 육지를 나타낸다. 항상 두 개 이상의 섬이 있는 데이터만 입력으로 주어진다.

---

#### 출력

첫째 줄에 가장 짧은 다리의 길이를 출력한다.

---

#### 예제 입력 1
~~~
10
1 1 1 0 0 0 0 1 1 1
1 1 1 1 0 0 0 0 1 1
1 0 1 1 0 0 0 0 1 1
0 0 1 1 1 0 0 0 0 1
0 0 0 1 0 0 0 0 0 1
0 0 0 0 0 0 0 0 0 1
0 0 0 0 0 0 0 0 0 0
0 0 0 0 1 1 0 0 0 0
0 0 0 0 1 1 1 0 0 0
0 0 0 0 0 0 0 0 0 0
~~~

#### 예제 출력 1
~~~
3
~~~

[출처](https://www.acmicpc.net/problem/2146)

---

### 문제풀이

이번 문제는 bfs 문제입니다.   

제가 이 문제를 푼 방법은 아래와 같습니다.   

1. bfs를 통해 모든 섬의 가장 자리의 좌표를 구한다.   

2. 각 섬의 가강 자리를 서로 비교해서 가장 값이 작은 값을 정답으로 제출한다.   

이 방법으로 제출했는데, 시간 초과 판정을 받았습니다.   

혹시 가장 작은 값이 1인 경우가 너무 많은 것이 문제인가 싶어서 예외 처리를 해줬는데, 바로 통과할 수 있었습니다.   

편법을 써서 통과한 것 같아서 다른 분들의 풀이를 찾아보니 다르게 푸는 방법이 있었습니다.   

1. dfs 혹은 bfs를 이용해서 모든 섬에 번호를 매긴다.

2. 각 섬의 모든 좌표를 queue에 넣고 bfs를 실행한다.

3. 다른 섬의 번호가 나오면 bfs를 종료하고 걸린 시간을 반환한다.

4. 걸린 시간 중에 가장 작은 시간을 정답으로 제출한다.

이전에 풀었던 문제에서도 느꼈는데, bfs를 할 때, 처음에 queue에 넣는 방식이 참신했습니다.   

다음에 풀때는 저도 같은 방법을 먼저 생각해봐야겠습니다.   

---

#### 나의 풀이

~~~python
from collections import deque

n = int(input())
country = [list(map(int, input().split())) for _ in range(n)]
edge = []
island = 0
min_bridge = int(10e9)
# 모든 섬을 탐색하면서 가장 자리의 좌표를 구함
def bfs(row, col):
    queue = deque([(row, col)])
    country[row][col] = 2

    while queue:
        # 가장자리 중복 탐색을 방지하기 위한 플래그
        edge_flag = False
        x, y = queue.popleft()

        for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
            nx, ny = x + dx, y + dy
            # 좌표를 벗어나지 않는 경우
            if 0 <= nx < n and 0 <= ny < n:
                # 아직 방문하지 않은 곳을 탐색
                if country[nx][ny] == 1:
                    country[nx][ny] = 2
                    queue.append((nx, ny))
                # 가장 자리인 경우, 그리고 아직 가장 자리 리스트에 좌표를 넣지 않은 경우
                if not edge_flag and not country[nx][ny]:
                    edge_flag = True
                    edge.append((island, x, y))
# 모든 좌표에서 bfs를 실행
for i in range(n):
    for j in range(n):
        if country[i][j] == 1:
            bfs(i, j)
            island += 1
# 모든 가장 자리 좌표에서 다른 섬과의 최단 거리를 구함
for i in range(len(edge)):
    for j in range(i + 1, len(edge)):
        # 같은 섬의 가장 자리끼리는 계산하지 않음
        if edge[i][0] != edge[j][0]:
            min_bridge = min(abs(edge[i][1]-edge[j][1]) + abs(edge[i][2]-edge[j][2]) - 1, min_bridge)
            # 최소 값이 1인 경우, 그 이하의 값이 나올수 없으므로 예외처리
            if min_bridge == 1:
                print(1)
                exit()
print(min_bridge)
~~~

---

#### 다른 사람의 풀이

~~~python
from collections import deque
import sys
sys.setrecursionlimit(10000000)
input=sys.stdin.readline
dx=[0,0,-1,1]
dy=[-1,1,0,0]
n=int(input())
arr=[list(map(int,input().split())) for i in range(n)]
visited=[[0]*n for i in range(n)]
cnt=1
ans=sys.maxsize

def dfs(x,y):
    global cnt
    arr[x][y]=cnt
    q[cnt].append([x,y])
    visited[x][y]=1
    for i in range(4):
        nx=x+dx[i]
        ny=y+dy[i]
        if 0<=nx<n and 0<=ny<n and visited[nx][ny]==0 and arr[nx][ny]:
            dfs(nx,ny)
def bfs(num):
    global ans
    dist=[[0]*n for i in range(n)]
    a=deque(q[num])
    while a:
        x,y=a.popleft()
        for i in range(4):
            nx=x+dx[i]
            ny=y+dy[i]
            if 0<=nx<n and 0<=ny<n:
                if arr[nx][ny]>0 and arr[nx][ny]!=num:
                    ans=min(ans,dist[x][y])
                    return 
                if arr[nx][ny]==0 and dist[nx][ny]==0:
                    dist[nx][ny]=dist[x][y]+1
                    a.append([nx,ny])
q=[[]]
for i in range(n):
    for j in range(n):
        if arr[i][j] and visited[i][j]==0:
            q.append([])
            dfs(i,j)
            cnt+=1

for i in range(1,cnt):
    bfs(i)
print(ans)
~~~
