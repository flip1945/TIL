# 아기 상어 (Gold 4)

### 문제

N×N 크기의 공간에 물고기 M마리와 아기 상어 1마리가 있다. 공간은 1×1 크기의 정사각형 칸으로 나누어져 있다. 한 칸에는 물고기가 최대 1마리 존재한다.   

아기 상어와 물고기는 모두 크기를 가지고 있고, 이 크기는 자연수이다. 가장 처음에 아기 상어의 크기는 2이고, 아기 상어는 1초에 상하좌우로 인접한 한 칸씩 이동한다.   

아기 상어는 자신의 크기보다 큰 물고기가 있는 칸은 지나갈 수 없고, 나머지 칸은 모두 지나갈 수 있다. 아기 상어는 자신의 크기보다 작은 물고기만 먹을 수 있다. 따라서, 크기가 같은 물고기는 먹을 수 없지만, 그 물고기가 있는 칸은 지나갈 수 있다.   

아기 상어가 어디로 이동할지 결정하는 방법은 아래와 같다.   

* 더 이상 먹을 수 있는 물고기가 공간에 없다면 아기 상어는 엄마 상어에게 도움을 요청한다.

* 먹을 수 있는 물고기가 1마리라면, 그 물고기를 먹으러 간다.

* 먹을 수 있는 물고기가 1마리보다 많다면, 거리가 가장 가까운 물고기를 먹으러 간다.

    * 거리는 아기 상어가 있는 칸에서 물고기가 있는 칸으로 이동할 때, 지나야하는 칸의 개수의 최솟값이다.

    * 거리가 가까운 물고기가 많다면, 가장 위에 있는 물고기, 그러한 물고기가 여러마리라면, 가장 왼쪽에 있는 물고기를 먹는다.

아기 상어의 이동은 1초 걸리고, 물고기를 먹는데 걸리는 시간은 없다고 가정한다. 즉, 아기 상어가 먹을 수 있는 물고기가 있는 칸으로 이동했다면, 이동과 동시에 물고기를 먹는다. 물고기를 먹으면, 그 칸은 빈 칸이 된다.   

아기 상어는 자신의 크기와 같은 수의 물고기를 먹을 때 마다 크기가 1 증가한다. 예를 들어, 크기가 2인 아기 상어는 물고기를 2마리 먹으면 크기가 3이 된다.   

공간의 상태가 주어졌을 때, 아기 상어가 몇 초 동안 엄마 상어에게 도움을 요청하지 않고 물고기를 잡아먹을 수 있는지 구하는 프로그램을 작성하시오.   

---

#### 입력

첫째 줄에 공간의 크기 N(2 ≤ N ≤ 20)이 주어진다.   

둘째 줄부터 N개의 줄에 공간의 상태가 주어진다. 공간의 상태는 0, 1, 2, 3, 4, 5, 6, 9로 이루어져 있고, 아래와 같은 의미를 가진다.   

* 0: 빈 칸

* 1, 2, 3, 4, 5, 6: 칸에 있는 물고기의 크기

* 9: 아기 상어의 위치

아기 상어는 공간에 한 마리 있다.

---

#### 출력

첫째 줄에 아기 상어가 엄마 상어에게 도움을 요청하지 않고 물고기를 잡아먹을 수 있는 시간을 출력한다.

---

#### 예제 입력 1
~~~
3
0 0 0
0 0 0
0 9 0
~~~

#### 예제 출력 1
~~~
0
~~~

#### 예제 입력 2
~~~
3
0 0 1
0 0 0
0 9 0
~~~

#### 예제 출력 2
~~~
3
~~~

#### 예제 입력 3
~~~
4
4 3 2 1
0 0 0 0
0 0 9 0
1 2 3 4
~~~

#### 예제 출력 3
~~~
14
~~~

#### 예제 입력 4
~~~
6
5 4 3 2 3 4
4 3 2 3 4 5
3 2 9 5 6 6
2 1 2 3 4 5
3 2 1 6 5 4
6 6 6 6 6 6
~~~

#### 예제 출력 4
~~~
60
~~~

#### 예제 입력 5
~~~
6
6 0 6 0 6 1
0 0 0 0 0 2
2 3 4 5 6 6
0 0 0 0 0 2
0 2 0 0 0 0
3 9 3 0 0 1
~~~

#### 예제 출력 5
~~~
48
~~~

#### 예제 입력 6
~~~
6
1 1 1 1 1 1
2 2 6 2 2 3
2 2 5 2 2 3
2 2 2 4 6 3
0 0 0 0 0 6
0 0 0 0 0 9
~~~

#### 예제 출력 6
~~~
39
~~~

출처 : https://www.acmicpc.net/problem/16236

---

### 문제풀이

이번 문제는 bfs 문제입니다.   

정해진 순서에 따라 물고기를 먹어야 하는데, 특이한 점이 있습니다.   

탐색 후, 동일한 거리에 있는 물고기는 가장 위쪽에 있는 물고기를 선택해야 하고, 동일한 높이라면 가장 왼쪽에 있는 물고기를 선택해야 합니다.   

저는 처음에 이 조건이 탐색의 순서를 의미하는 것이라고 생각했습니다.   

그러나 탐색 순서를 상하좌우 순으로 하는 것과 조건은 관계가 없었습니다.   

이 조건의 진짜 의미는 아기 상어의 위치에서 bfs를 실행하고, 먹을 수 있는 모든 먹이를 탐색해야 한다는 것입니다.   

모든 먹이를 탐색해야 조건에 맞는 물고기의 좌표를 알 수 있습니다.   

조건에 맞는 물고기를 찾는 것은 정렬을 통해 가능합니다.   

저는 정렬을 사용했지만 heap을 사용해 문제를 푸신 분도 있었습니다.   

그리고 또 예외 처리를 해야 하는 점이 있는데, 아기 상어의 크기를 제한해야 한다는 점입니다.   

아기 상어의 크기가 9를 넘어가면 자기 자신을 먹이로 인식하기 때문에 예외처리를 통해 해결해줘야 합니다.   

---

#### 나의 풀이

~~~python
from collections import deque

n = int(input())
borad = [list(map(int, input().split())) for _ in range(n)]
visited = [[0] * n for _ in range(n)]
answer = 0

cur_x = 0
cur_y = 0
cur_size = 2
# 처음 시작 위치를 설정
for i in range(n):
    for j in range(n):
        if borad[i][j] == 9:
            cur_x = i
            cur_y = j
            break

def bfs(row, col, size):
    queue = deque([(row, col)])
    new_visited = [v[:] for v in visited]
    new_visited[row][col] = 1
    answer = []

    while queue:
        x, y = queue.popleft()
        # 먹을 수 있는 물고기를 발견하면 정답 리스트에 저장
        if 0 < borad[x][y] < size:
            answer.append((new_visited[x][y] - 1, x, y))

        for dx, dy in [(-1, 0), (0, -1), (1, 0), (0, 1)]:
            dx, dy = dx + x, dy + y
            # 아직 방문하지 않은 장소이고, 해당 장소의 물고기의 크기가 아기 상어의 크기 이하인 곳만 탐색
            if 0 <= dx < n and 0 <= dy < n and not new_visited[dx][dy] and borad[dx][dy] <= size:
                # 방문리스트에 해당 위치에 도달하기까지의 거리를 저장
                new_visited[dx][dy] = new_visited[x][y] + 1
                queue.append((dx, dy))
    # 정렬을 통해 먹어야 할 물고기를 선택(같은 거리라면, 위에 있을수록, 그래도 같다면 왼쪽에 있는 물고기를 선택)
    answer.sort()
    # 먹을 물고기가 있다면
    if answer:
        x = answer[0][1]
        y = answer[0][2]
        # 상어의 위치를 변경
        borad[row][col] = 0
        borad[x][y] = 9
        # 먹이를 먹기까지의 시간과 좌표를 반환
        return answer[0][0], x, y
    # 먹을 물고기가 없다면 0을 반환
    return 0, 0, 0
# 모든 먹이를 먹을 때까지 걸리는 시간을 저장하는 변수
sec = 0
# 몇 마리의 물고기를 먹어야 크기가 커지는 지 저장하는 변수
feed = cur_size

while True:
    # 탐색 실행
    cur_sec, cur_x, cur_y = bfs(cur_x, cur_y, cur_size)
    # 탐색 결과 더 이상 먹을 물고기가 없다면 반복문 종료
    if not cur_sec:
        break
    # 걸리는 시간 갱신
    sec += cur_sec
    # 먹이를 -1
    feed -= 1
    # 먹이를 먹어서 크기가 커지는 경우
    if not feed:
        # 크기가 7 이상이면 7로 고정시키도록 예외 처리
        cur_size = cur_size + 1 if cur_size < 7 else 7
        # 먹이의 크기를 
        feed = cur_size

print(sec)
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/19179639

~~~python
from heapq import*
n=int(input())
a=[list(map(int,input().split()))for _ in range(n)]
pq=[]
visit=[[0]*n for _ in range(n)]
for i in range(n):
    for j in range(n):
        if a[i][j]==9:
            a[i][j]=0
            heappush(pq,(0,i,j))
            break
size, eated, res = 2, 0, 0
while pq:
    d, x, y = heappop(pq)
    visit[x][y]=1
    if 0 < a[x][y] < size:
        a[x][y]=0
        eated+=1
        if eated==size:
            eated=0
            size+=1
        pq=[]
        visit=[[0]*n for _ in range(n)]
        res += d
        d=0
    for dx, dy in [(-1, 0), (0, 1), (1, 0), (0, -1)]:
        nx, ny = x+dx, y+dy
        if nx<0 or ny<0 or nx>n-1 or ny>n-1 or visit[nx][ny] or\
           a[nx][ny] > size: continue
        heappush(pq, (d+1, nx, ny))
        visit[nx][ny]=1
print(res)
~~~
