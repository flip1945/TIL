# 전쟁 - 전투(Silver 1)

### 문제

전쟁은 어느덧 전면전이 시작되었다. 결국 전투는 난전이 되었고, 우리 병사와 적국 병사가 섞여 싸우게 되었다.   

그러나 당신의 병사들은 하얀 옷을 입고, 적국의 병사들은 파란옷을 입었기 때문에 서로가 적인지 아군인지는 구분할 수 있다.   

문제는, 같은 팀의 병사들은 모이면 모일수록 강해진다는 사실이다.   

N명이 뭉쳐있을 때는 N^2의 위력을 낼 수 있다. 과연 지금 난전의 상황에서는 누가 승리할 것인가? 단, 같은 팀의 병사들이 대각선으로만 인접한 경우는 뭉쳐 있다고 보지 않는다.   

---

#### 입력

첫째 줄에는 전쟁터의 가로 크기 N, 세로 크기 M(1 ≤ N, M ≤ 100)이 주어진다. 그 다음 두 번째 줄에서 M+1번째 줄에는 각각 (X, Y)에 있는 병사들의 옷색이 띄어쓰기 없이 주어진다. 모든 자리에는 병사가 한 명 있다. (B는 파랑, W는 흰색이다.)   

---

#### 출력

첫 번째 줄에 당신의 병사의 위력의 합과 적국의 병사의 위력의 합을 출력한다.   

---

#### 예제 입력 1
~~~
5 5
WBWWW
WWWWW
BBBBB
BBBWW
WWWWW 
~~~

#### 예제 출력 1
~~~
130 65
~~~

[출처](https://www.acmicpc.net/problem/1303)

---

### 문제풀이

이번 문제는 dfs 혹은 bfs를 사용해서 푸는 문제입니다.   

두 가지 방법 모두 사용 가능한데, 저는 연결된 그래프의 최대 크기를 구할 때는 dfs를 사용하는 편이어서 dfs로 문제를 풀었습니다.   

dfs로 문제를 풀다보니 재귀 호출을 사용할 때, 가지고 가야 하는 변수들 있어서 매개 변수 커졌습니다.   

solution 함수가 안에 dfs함수를 선언하면 매개 변수의 수를 줄일 수 있을 것 같은데, 다음에는 그런 방식으로 풀어봐야 겠습니다.   

그리고 bfs로 푼 방법이 궁금해서 다른 분의 풀이를 아래에 첨부합니다.   

---

#### 나의 풀이

~~~python
def solution(n, m, soldiers):
    # 방문 리스트 생성
    visited = [[False for col in range(n)] for row in range(m)]
    answer = {"W": [], "B": []}
    # 모든 병사 확인
    for row in range(m):
        for col in range(n):
            # 이미 방문한 병사라면 생략
            if visited[row][col]:
                continue
            soldier = soldiers[row][col]
            # 정답 리스트에 병사들의 숫자 추가
            answer[soldier].append(dfs(row, col, n, m, soldiers, visited, soldier, 0))
    
    print(sum([ans**2 for ans in answer["W"]]), sum([ans**2 for ans in answer["B"]]))

# 그래프를 생성하는 함수
def input_soldiers(n):
    soldiers = [list(input()) for _ in range(n)]
    return soldiers


def dfs(x, y, n, m, soldiers, visited, t, result):
    # 이미 방문 했거나 확인하고 있는 병사의 종류가 다르다면 바로 종료
    if visited[x][y] or soldiers[x][y] != t:
        return result
    # 현재 확인하는 병사를 방문 처리
    visited[x][y] = True
    result += 1
    # 4가지 방향으로 dfs 실행
    for way in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
        dx, dy = x + way[0], y + way[1]
        # 좌표를 벗어나는 경우는 생략
        if dx < 0 or dx >= m or dy < 0 or dy >= n:
            continue
        
        result = dfs(dx, dy, n, m, soldiers, visited, t, result)

    return result


if __name__ == "__main__":
    n, m = map(int, input().split())
    s = input_soldiers(m)
    solution(n, m, s)
~~~

---

#### 다른 사람의 풀이

출처 : https://velog.io/@eujin/Algorithm-%EB%B0%B1%EC%A4%80-1303-%EC%A0%84%EC%9F%81-%EC%A0%84%ED%88%AC-DFSBFS-Python

~~~python
from collections import deque

m,n = map(int, input().split())
graph = [list(input().strip()) for _ in range(n)]
w,b=0,0

dx = [-1,1,0,0]
dy = [0,0,-1,1]

def bfs(x,y,team):
    queue = deque()
    queue.append((x,y))
    count = 0
    graph[x][y] == 0
    while queue:
        x, y = queue.popleft()
        for i in range(4):
            nx = x+dx[i]
            ny = y+dy[i]
            if nx<0 or ny<0 or nx>=n or ny>=m:
                continue
            if graph[nx][ny] != 0 and graph[nx][ny] == team:
                queue.append((nx,ny))
                graph[nx][ny] = 0
                count += 1
    return (1 if count==0 else count)

for i in range(n):
    for j in range(m):
        if graph[i][j] != 0:
            if graph[i][j] == 'W':
                w += bfs(i,j,graph[i][j])**2
            else:
                b += bfs(i,j,graph[i][j])**2
print(w,b)
~~~
