# 연구소(Gold 5)

### 문제

인체에 치명적인 바이러스를 연구하던 연구소에서 바이러스가 유출되었다. 다행히 바이러스는 아직 퍼지지 않았고, 바이러스의 확산을 막기 위해서 연구소에 벽을 세우려고 한다.   

연구소는 크기가 N×M인 직사각형으로 나타낼 수 있으며, 직사각형은 1×1 크기의 정사각형으로 나누어져 있다. 연구소는 빈 칸, 벽으로 이루어져 있으며, 벽은 칸 하나를 가득 차지한다.   

일부 칸은 바이러스가 존재하며, 이 바이러스는 상하좌우로 인접한 빈 칸으로 모두 퍼져나갈 수 있다. 새로 세울 수 있는 벽의 개수는 3개이며, 꼭 3개를 세워야 한다.   

예를 들어, 아래와 같이 연구소가 생긴 경우를 살펴보자.   

~~~
2 0 0 0 1 1 0
0 0 1 0 1 2 0
0 1 1 0 1 0 0
0 1 0 0 0 0 0
0 0 0 0 0 1 1
0 1 0 0 0 0 0
0 1 0 0 0 0 0
~~~

이때, 0은 빈 칸, 1은 벽, 2는 바이러스가 있는 곳이다. 아무런 벽을 세우지 않는다면, 바이러스는 모든 빈 칸으로 퍼져나갈 수 있다.   

2행 1열, 1행 2열, 4행 6열에 벽을 세운다면 지도의 모양은 아래와 같아지게 된다.   

~~~
2 1 0 0 1 1 0
1 0 1 0 1 2 0
0 1 1 0 1 0 0
0 1 0 0 0 1 0
0 0 0 0 0 1 1
0 1 0 0 0 0 0
0 1 0 0 0 0 0
~~~

바이러스가 퍼진 뒤의 모습은 아래와 같아진다.   

~~~
2 1 0 0 1 1 2
1 0 1 0 1 2 2
0 1 1 0 1 2 2
0 1 0 0 0 1 2
0 0 0 0 0 1 1
0 1 0 0 0 0 0
0 1 0 0 0 0 0
~~~

벽을 3개 세운 뒤, 바이러스가 퍼질 수 없는 곳을 안전 영역이라고 한다. 위의 지도에서 안전 영역의 크기는 27이다.   

연구소의 지도가 주어졌을 때 얻을 수 있는 안전 영역 크기의 최댓값을 구하는 프로그램을 작성하시오.   

---

#### 입력

첫째 줄에 지도의 세로 크기 N과 가로 크기 M이 주어진다. (3 ≤ N, M ≤ 8)   

둘째 줄부터 N개의 줄에 지도의 모양이 주어진다. 0은 빈 칸, 1은 벽, 2는 바이러스가 있는 위치이다. 2의 개수는 2보다 크거나 같고, 10보다 작거나 같은 자연수이다.   

빈 칸의 개수는 3개 이상이다.   

---

#### 출력

첫째 줄에 얻을 수 있는 안전 영역의 최대 크기를 출력한다.   

---

#### 예제 입력 1
~~~
7 7
2 0 0 0 1 1 0
0 0 1 0 1 2 0
0 1 1 0 1 0 0
0 1 0 0 0 0 0
0 0 0 0 0 1 1
0 1 0 0 0 0 0
0 1 0 0 0 0 0
~~~

#### 예제 출력 1
~~~
27
~~~

#### 예제 입력 2
~~~
4 6
0 0 0 0 0 0
1 0 0 0 0 2
1 1 1 0 0 2
0 0 0 0 0 2
~~~

#### 예제 출력 2
~~~
9
~~~

#### 예제 입력 3
~~~
8 8
2 0 0 0 0 0 0 2
2 0 0 0 0 0 0 2
2 0 0 0 0 0 0 2
2 0 0 0 0 0 0 2
2 0 0 0 0 0 0 2
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
~~~

#### 예제 출력 3
~~~
3
~~~

[출처](https://www.acmicpc.net/problem/14502)

---

### 문제풀이

이번 문제는 bfs + 완전탐색 문제입니다.   

이 문제를 풀 때 핵심은 모든 좌표를 확인하는 것이 아니라 바이러스가 있는 위치에서만 확인하면 된다는 점입니다.   

바이러스가 존재하는 위치의 좌표를 모두 모은 virus라는 리스트를 만든고 바이러스의 숫자만큼만 bfs를 실행하면 됩니다.   

저는 처음에 모든 좌표를 확인하도록 구현했습니다.   

그렇게 하니 시간 초과 판정을 받아서 PyPy3로도 제출해봤고, 같은 로직으로 통과했습니다.   

PyPy3 말고 python으로 제출하신 분들의 코드를 보니 모든 좌표를 확인하지 않고, 바이러스가 있는 곳의 좌표에서만 bfs를 실행하는 것을 알게 됐습니다.   

분명히 이전에 풀었던 문제 중에 비슷한 유형의 문제가 있었는 데, 오랜만에 bfs 문제를 풀다보니 까먹었습니다.   

주기적으로 연습이 필요하다는 것을 깨달았습니다.   

그리고 bfs문제를 오랜만에 풀다보니 좌표가 겹치는 부분의 처리를 까먹어서 고생했습니다. 다음부터 유의해야겠습니다.   

마지막으로 python에서 리스트를 복사할 때, 복사한 리스트에서 추가가 이루어지지 않고, 재할당만 이루어진다면 slice를 이용한 복사가 더 빠르다는 걸 처음 알게 됐습니다.   

똑같은 시간이 걸릴것 같지만 slice로 복사한 리스트는 얕은 복사의 일종이라 복사 속도가 더 빨랐습니다.   

이 문제에서는 deepcopy를 사용하는 것보다 slice를 이용한 복사를 사용하면 정답 제출시간이 절반으로 줄었습니다.   

---

#### 나의 풀이

~~~python
from collections import deque

n, m = map(int, input().split())
lab = [list(map(int, input().split())) for _ in range(n)]
# 비어있는 공간의 좌표를 담을 리스트
empty = []
# 바이러스가 있는 공간의 좌표를 담을 리스트
virus = []
# 안전 지역의 최대값을 저장할 변수
max_zero = 0

for row in range(n):
    for col in range(m):
        if not lab[row][col]:
            # 비어있는 공간의 좌표를 리스트에 입력
            empty.append((row, col))
        elif lab[row][col] == 2:
            # 바이러스가 있는 공간의 좌표를 리스트에 입력
            virus.append((row, col))

def bfs():
    # slice를 이요한 리스트 복사
    new_lab = [l[:] for l in lab]
    # 바이러스의 숫자만큼 bfs를 실행
    for row, col in virus:
        # bfs를 위한 queue 생성
        queue = deque([(row, col)])
        while queue:
            x, y = queue.popleft()
            # 상하좌우 4방향 탐색
            for dx, dy in [(0, -1), (0, 1), (-1, 0), (1, 0)]:
                dx, dy = dx + x, dy + y
                # 좌표에서 벗어나지 않고, 비어있는 공간이라면 바이러스 전염
                if 0 <= dx < n and 0 <= dy < m and not new_lab[dx][dy]:
                    # 다음 좌표를 미리 수정함으로써 중복 좌표 탐색 방지!!!
                    new_lab[dx][dy] = 2
                    queue.append((dx, dy))
    # 안전 지대의 총합을 반환
    return sum([n.count(0) for n in new_lab])
# 재귀를 이용한 완전 탐색(itertools의 Conbination을 사용해도 됨)
def back_tracking(cnt, pos):
    global max_zero
    # 벽을 3개 세우면 bfs 실행
    if cnt == 3:
        # 현재까지의 확인한 안전 지대 최대값과 지금 벽을 세우고 확인한 안전 지대의 값을 비교
        max_zero = max(max_zero, bfs())
        return
    # back tracking으로 모든 조합을 확인
    for i in range(pos, len(empty)):
        x, y = empty[i]
        # 선택한 좌표에 벽을 세움
        lab[x][y] = 1
        back_tracking(cnt + 1, i + 1)
        # 선택한 좌표에 벽을 지움
        lab[x][y] = 0

back_tracking(0, 0)
print(max_zero)
~~~

---

#### 다른 사람의 풀이

~~~python
from itertools import product, combinations


N, M = map(int, input().split())
B = [list(map(int, input().split())) for _ in range(N)]

points = [(i, j) for i, j in product(range(N), range(M)) if B[i][j] == 0]
stack_o = [(i, j) for i, j in product(range(N), range(M)) if B[i][j] == 2]
d = [(1, 0), (0, 1), (-1, 0), (0, -1)]
ans = 0

for wall in combinations(points, 3):
    b = [row[:] for row in B]
    for y, x in wall:
        b[y][x] = 1

    stack = stack_o.copy()

    while stack:
        i, j = stack.pop()

        for dx, dy in d:
            ni, nj = i + dy, j + dx
            if 0 <= ni < N and 0 <= nj < M and b[ni][nj] == 0:
                b[ni][nj] = 2
                stack.append((ni, nj))

    ans = max(ans, sum(row.count(0) for row in b))

print(ans)
~~~
