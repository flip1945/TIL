# 일루미네이션 (Gold 5)

### 문제

부유한 집안의 상속자 상근이네 집은 그림과 같이 1미터의 정육각형이 붙어있는 상태이다. 크리스마스가 다가오기 때문에, 여자친구에게 잘 보이기 위해 상근이는 건물의 벽면을 조명으로 장식하려고 한다. 외부에 보이지 않는 부분에 조명을 장식하는 것은 낭비라고 생각했기 때문에, 밖에서 보이는 부분만 장식하려고 한다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/building.png" width="522px"/>

위의 그림은 상공에서 본 상근이네 집의 건물 배치이다. 정육각형 안의 숫자는 좌표를 나타낸다. 여기서 회색 정육각형은 건물의 위치이고, 흰색은 건물이 없는 곳이다. 위에서 붉은 색 선으로 표시된 부분이 밖에서 보이는 벽이고, 이 벽에 조명을 장식할 것이다. 벽의 총 길이는 64미터이다.

상근이네 집의 건물 위치 지도가 주어졌을 때, 조명을 장식할 벽면의 길이의 합을 구하는 프로그램을 작성하시오. 지도의 바깥은 자유롭게 왕래 할 수 있는 곳이고, 인접한 건물 사이는 통과할 수 없다.

---

#### 입력

첫째 줄에 두 개의 정수 W와 H가 주어진다. (1 ≤ W, H ≤ 100) 다음 H줄에는 상근이네 집의 건물 배치가 주어진다. i+1줄에는 W개의 정수가 공백으로 구분되어 있다. j번째 (1 ≤ j ≤ w) 정수의 좌표는 (j, i)이며, 건물이 있을 때는 1이고, 없을 때는 0이다. 주어지는 입력에는 건물이 적어도 하나 있다.

지도는 다음과 같은 규칙에 의해 만들어졌다.

* 지도의 가장 왼쪽 위에 있는 정육각형의 좌표는 (1,1)이다.
* (x,y)의 오른족에 있는 정육각형의 좌표는 (x+1,y)이다.
* y가 홀수 일 때, 아래쪽에 있는 정육각형의 좌표는 (x,y+1)이다.
* y가 짝수 일 때, 오른쪽 아래에 있는 정육각형의 좌표는 (x,y+1)이다.

---

#### 출력

조명을 장식하는 벽면의 길이의 합을 출력한다.

출처 : https://www.acmicpc.net/problem/5547

---

### 문제풀이

이번 문제는 그래프 탐색 문제입니다.

특이하게도 그림이 6각형 형태로 구성돼있고 둘레의 총합을 구해야 합니다.

6각형 형태이기 때문에 다음 노드를 탐색하는 방향을 2가지로 구분해서 만들어줘야 한다는 게 까다로운 점이었습니다.

그리고 내부에 있는 공간은 무시해야 한다는 점도 처리하기 어려웠습니다.

제가 처리한 방법은 건물 내부에서 외부와 맞닿는 점의 개수를 둘레로 구하는 방법이었습니다. 해당 방법으로 처리하기 위해 건물 내부에 있는 빈공간을 건물로 간주하는 처리를 추가해서 문제를 해결했스니다.

문제를 풀고 다른 분들의 풀이를 찾아보니 건물 외부에서 건물과 맞닿는 점의 개수를 구해 문제를 해결할 수 있다는 것을 깨달았습니다. 해당 방식은 건물 내부에 있는 빈공간에 대해서 신경쓸 필요가 없기 때문에 풀이가 더 간결하다는 점이 좋았습니다.

---

#### 나의 풀이

~~~python
from collections import deque

w, h = map(int, input().split())
hexagon = [list(map(int, input().split())) for _ in range(h)]


def bfs(graph, start, width, height):
    graph[start[0]][start[1]] = -1
    result = [start]
    queue = deque([start])
    is_inside = True

    while queue:
        current = queue.popleft()

        directions = [[-1, -1], [-1, 0], [0, 1], [1, 0], [1, -1], [0, -1]] if current[0] % 2 else [[-1, 0], [-1, 1], [0, 1], [1, 1], [1, 0], [0, -1]]

        for d in directions:
            row = current[0] + d[0]
            col = current[1] + d[1]
            if row < 0 or row >= height or col < 0 or col >= width:
                is_inside = False
                continue
            if not graph[row][col]:
                result.append([row, col])
                queue.append([row, col])
                graph[row][col] = -1

    if is_inside:
        for row, col in result:
            graph[row][col] = 1


for i in range(h):
    for j in range(w):
        if not hexagon[i][j]:
            bfs(hexagon, [i, j], w, h)

answer = 0
for i in range(h):
    for j in range(w):
        if hexagon[i][j] == 1:
            direction = [[-1, -1], [-1, 0], [0, 1], [1, 0], [1, -1], [0, -1]] if i % 2 else [[-1, 0], [-1, 1], [0, 1], [1, 1], [1, 0], [0, -1]]
            answer += 6 - sum(1 for d in direction if (0 <= i + d[0] < h) and (0 <= j + d[1] < w) and (hexagon[i+d[0]][j+d[1]] == 1))

print(answer)
~~~

#### 다른 사람의 풀이

https://www.acmicpc.net/source/28843238
