# 섬의 개수 (Silver 2)

### 문제

사각형으로 이루어져 있는 섬과 바다 지도가 주어진다. 섬의 개수를 세는 프로그램을 작성하시오.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/island.png">

한 정사각형과 가로, 세로 또는 대각선으로 연결되어 있는 사각형은 걸어갈 수 있는 사각형이다. 

두 정사각형이 같은 섬에 있으려면, 한 정사각형에서 다른 정사각형으로 걸어서 갈 수 있는 경로가 있어야 한다. 지도는 바다로 둘러싸여 있으며, 지도 밖으로 나갈 수 없다.  

---

#### 입력

입력은 여러 개의 테스트 케이스로 이루어져 있다. 각 테스트 케이스의 첫째 줄에는 지도의 너비 w와 높이 h가 주어진다. w와 h는 50보다 작거나 같은 양의 정수이다.

둘째 줄부터 h개 줄에는 지도가 주어진다. 1은 땅, 0은 바다이다.

입력의 마지막 줄에는 0이 두 개 주어진다.

---

#### 출력

각 테스트 케이스에 대해서, 섬의 개수를 출력한다.

---

#### 예제 입력 1
~~~
1 1
0
2 2
0 1
1 0
3 2
1 1 1
1 1 1
5 4
1 0 1 0 0
1 0 0 0 0
1 0 1 0 1
1 0 0 1 0
5 4
1 1 1 0 1
1 0 1 0 1
1 0 1 0 1
1 0 1 1 1
5 5
1 0 1 0 1
0 0 0 0 0
1 0 1 0 1
0 0 0 0 0
1 0 1 0 1
0 0
~~~

#### 예제 출력 1
~~~
0
1
1
3
1
9
~~~

출처 : https://www.acmicpc.net/problem/4963

---

### 문제풀이

이번 문제는 그래프 탐색 문제입니다.

bfs 혹은 dfs로 문제를 해결하면 되는데, 저는 dfs를 이용해 문제를 풀었습니다.

전체 컴포넌트의 개수를 물어보는 문제로 탐색 횟수를 정답으로 제출하면됩니다.

이 문제에서 특이했던 점은 탐색해야 할 방향이 4개가 아니라 대각선이 포함된 8개 였던 점입니다.

dfs를 실행할 때, 8개 방향 모두 탐색해주면 정답을 맞출 수 있습니다.

---

#### 나의 풀이

~~~python
def dfs(x, y):
    stack = [(x, y)]
    while stack:
        x, y = stack.pop()
        for dx, dy in [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]:
            nx, ny = x + dx, y + dy
            if 0 <= nx < h and 0 <= ny < w and land[nx][ny]:
                stack.append((nx, ny))
                land[nx][ny] = 0

while True:
    w, h = map(int, input().split())
    if w + h == 0:
        break
    land = [list(map(int, input().split())) for _ in range(h)]
    answer = 0
    for row in range(h):
        for col in range(w):
            if land[row][col]:
                dfs(row, col)
                answer += 1
    print(answer)
~~~
