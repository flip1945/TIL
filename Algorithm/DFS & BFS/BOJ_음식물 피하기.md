# 음식물 피하기 (Silver 1)

### 문제

코레스코 콘도미니엄 8층은 학생들이 3끼의 식사를 해결하는 공간이다. 그러나 몇몇 비양심적인 학생들의 만행으로 음식물이 통로 중간 중간에 떨어져 있다. 이러한 음식물들은 근처에 있는 것끼리 뭉치게 돼서 큰 음식물 쓰레기가 된다.    

이 문제를 출제한 선생님은 개인적으로 이러한 음식물을 실내화에 묻히는 것을 정말 진정으로 싫어한다. 참고로 우리가 구해야 할 답은 이 문제를 낸 조교를 맞추는 것이 아니다.    

통로에 떨어진 음식물을 피해가기란 쉬운 일이 아니다. 따라서 선생님은 떨어진 음식물 중에 제일 큰 음식물만은 피해 가려고 한다.   

선생님을 도와 제일 큰 음식물의 크기를 구해서 “10ra"를 외치지 않게 도와주자.   

---

#### 입력

첫째 줄에 통로의 세로 길이 N(1 ≤ N ≤ 100)과 가로 길이 M(1 ≤ M ≤ 100) 그리고 음식물 쓰레기의 개수 K(1 ≤ K ≤ 10,000)이 주어진다.  그리고 다음 K개의 줄에 음식물이 떨어진 좌표 (r, c)가 주어진다.   

좌표 (r, c)의 r은 위에서부터, c는 왼쪽에서부터가 기준이다.   

---

#### 출력

첫째 줄에 음식물 중 가장 큰 음식물의 크기를 출력하라.   

---

#### 예제 입력 1
~~~
3 4 5
3 2
2 2
3 1
2 3
1 1
~~~

#### 예제 출력 1
~~~
4
~~~

---

#### 힌트

\# . . .   
. \# \# .   
\# \# . .   
  
위와 같이 음식물이 떨어져있고 제일큰 음식물의 크기는 4가 된다. (인접한 것은 붙어서 크게 된다고 나와 있음. 대각선으로는 음식물 끼리 붙을수 없고 상하좌우로만 붙을수 있다.)   

출처 : https://www.acmicpc.net/problem/1743

---

### 문제풀이

이번 문제는 dfs, bfs 둘 중 하나를 사용해서 푸는 문제입니다.   

저는 dfs를 사용해서 풀었습니다. 그런데 코드를 잘 짠 것 같은데, 계속 실패했습니다.   

이유를 확인해 보니 재귀 호출 제한 런타임 에러가 발생해서 그랬습니다.   

그래서 재귀 호출 제한을 늘려줬더니 바로 통과했습니다.   

다음부터는 dfs를 사용할 때, 주의해야 겠습니다.   

---

#### 나의 풀이

~~~python
import sys
sys.setrecursionlimit(10000)

n, m, k = map(int, input().split())
# 음식물 리스트 생성
food = [["." for col in range(m)] for row in range(n)]
# 방문 리스트 생성
visited = [[True for col in range(m)] for row in range(n)]
result = []
# 음식물 리스트와 방문 리스트를 재설정
for i in range(k):
    x, y = map(int, input().split())
    food[x-1][y-1] = "#"
    visited[x-1][y-1] = False

def dfs(row, col, scale):
    # 이미 방문한 곳이라면 생략
    if visited[row][col]:
        return scale
    # 결과 반환할 변수의 크기를 1증가
    scale += 1
    # 현재 좌표를 방문처리
    visited[row][col] = True
    # 4가지 방향을 모두 확인
    for way in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
        dr, dc = row + way[0], col + way[1]
        # 좌표를 벗어나는 경우 생략
        if dr < 0 or dr >= n or dc < 0 or dc >= m:
            continue
        scale = dfs(dr, dc, scale)

    return scale
# 모든 좌표를 dfs 탐색
for row in range(n):
    for col in range(m):
        result.append(dfs(row, col, 0))
# 탐색 결과 중 가장 큰 음식물을 출력
print(max(result))
~~~
