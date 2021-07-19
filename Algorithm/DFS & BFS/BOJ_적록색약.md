# 적록색약(Gold 5)

### 문제

적록색약은 빨간색과 초록색의 차이를 거의 느끼지 못한다. 따라서, 적록색약인 사람이 보는 그림은 아닌 사람이 보는 그림과는 좀 다를 수 있다.   

크기가 N×N인 그리드의 각 칸에 R(빨강), G(초록), B(파랑) 중 하나를 색칠한 그림이 있다. 그림은 몇 개의 구역으로 나뉘어져 있는데, 구역은 같은 색으로 이루어져 있다. 또, 같은 색상이 상하좌우로 인접해 있는 경우에 두 글자는 같은 구역에 속한다. (색상의 차이를 거의 느끼지 못하는 경우도 같은 색상이라 한다)   

예를 들어, 그림이 아래와 같은 경우에   

~~~
RRRBB
GGBBB
BBBRR
BBRRR
RRRRR
~~~

적록색약이 아닌 사람이 봤을 때 구역의 수는 총 4개이다. (빨강 2, 파랑 1, 초록 1) 하지만, 적록색약인 사람은 구역을 3개 볼 수 있다. (빨강-초록 2, 파랑 1)   

그림이 입력으로 주어졌을 때, 적록색약인 사람이 봤을 때와 아닌 사람이 봤을 때 구역의 수를 구하는 프로그램을 작성하시오.   

---

#### 입력

첫째 줄에 N이 주어진다. (1 ≤ N ≤ 100)   

둘째 줄부터 N개 줄에는 그림이 주어진다.   

---

#### 출력

적록색약이 아닌 사람이 봤을 때의 구역의 개수와 적록색약인 사람이 봤을 때의 구역의 수를 공백으로 구분해 출력한다.   

---

#### 예제 입력 1
~~~
5
RRRBB
GGBBB
BBBRR
BBRRR
RRRRR
~~~

#### 예제 출력 1
~~~
4 3
~~~

[출처](https://www.acmicpc.net/problem/10026)

---

### 문제풀이

이번 문제는 dfs 혹은 bfs로 풀 수 있는 문제입니다.   

저는 dfs로 이 문제를 풀었고, 총 몇개의 영역이 있는지 구하면 됩니다.   

총 2번 모든 영역의 개수를 구해야 하는데, 색약이 아닌 사람이 본 영역과 색약인 사람이 본 영역 2가지 입니다.   

dfs를 실행할 때, 색약인 사람과 아닌 사람의 영역 2가지를 구분하도록 코드를 구현해 주면 쉽게 풀 수 있는 문제입니다.   

---

#### 나의 풀이

~~~python
import sys
sys.setrecursionlimit(10**6)
n = int(input())
board = [list(input()) for _ in range(n)]
answer = []

def dfs(x, y, color):
    visited[x][y] = True
    for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
        nx, ny = x + dx, y + dy
        if 0 <= nx < n and 0 <= ny < n and not visited[nx][ny] and board[nx][ny] in color:
            dfs(nx, ny, color)

for i in range(2):
    visited = [[False] * n for _ in range(n)]
    cnt = 0
    for row in range(n):
        for col in range(n):
            if not visited[row][col]:
                # 색약인 사람과 아닌 사람은 확인하는 영역을 다르게 구성
                c = ["R", "G"] if i == 1 and board[row][col] in ["R", "G"] else board[row][col]
                dfs(row, col, c)
                cnt += 1
    answer.append(cnt)

print(*answer)
~~~

---

#### 다른 사람의 풀이

~~~python
import sys
input= sys.stdin.readline
sys.setrecursionlimit(10000)

n = int(input())
g = [input()[:n] for i in range(n)]
rgb ={'R':0, 'G':2, 'B':1}

dx =[0,0,1,-1]
dy = [1,-1,0,0]

def floodfill(mod):
	global visit
	visit = [[0 for i in range(n)]for j in range(n)]
	cnt = 0
	for i in range(n):
		for j in range(n):
			if not visit[i][j]:
				dfs(i,j,mod)
				cnt+=1
	return cnt


def dfs(row,col,mod):
	global visit
	visit[row][col] =1
	for i in range(4):
		r = row + dx[i]
		c = col + dy[i]
		if r<0 or r>=n or c<0 or c>=n:
			continue
		if not visit[r][c]  and ((rgb[g[r][c]]%mod) == (rgb[g[row][col]]%mod)):
			dfs(r,c,mod)


print(floodfill(3), floodfill(2))
~~~
