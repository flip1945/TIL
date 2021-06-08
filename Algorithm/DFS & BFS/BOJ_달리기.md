# 달리기(Platinum 2)

### 문제

진영이는 다이어트를 위해 N×M 크기의 체육관을 달리려고 한다. 체육관은 1×1 크기의 칸으로 나누어져 있고, 칸은 빈 칸 또는 벽이다. x행 y열에 있는 칸은 (x, y)로 나타낸다.   

매 초마다 진영이는 위, 아래, 오른쪽, 왼쪽 중에서 이동할 방향을 하나 고르고, 그 방향으로 최소 1개, 최대 K개의 빈 칸을 이동한다.   

시작점 (x1, y1)과 도착점 (x2, y2)가 주어졌을 때, 시작점에서 도착점으로 이동하는 최소 시간을 구해보자.    

---

#### 입력

첫째 줄에 체육관의 크기 N과 M, 1초에 이동할 수 있는 칸의 최대 개수 K가 주어진다.   

둘째 줄부터 N개의 줄에는 체육관의 상태가 주어진다. 체육관의 각 칸은 빈 칸 또는 벽이고, 빈 칸은 '.', 벽은 '#'으로 주어진다.   

마지막 줄에는 네 정수 x1, y1, x2, y2가 주어진다. 두 칸은 서로 다른 칸이고, 항상 빈 칸이다.   

---

#### 출력

(x1, y1)에서 (x2, y2)로 이동하는 최소 시간을 출력한다. 이동할 수 없는 경우에는 -1을 출력한다.   

---

#### 제한

* 2 ≤ N, M ≤ 1,000

* 1 ≤ K ≤ 1,000

* 1 ≤ x1, x2 ≤ N

* 1 ≤ y1, y2 ≤ M

---

#### 예제 입력 1
~~~
3 4 4
....
###.
....
1 1 3 1
~~~

#### 예제 출력 1
~~~
3
~~~

#### 예제 입력 2
~~~
3 4 1
....
###.
....
1 1 3 1
~~~

#### 예제 출력 2
~~~
8
~~~

#### 예제 입력 3
~~~
2 2 1
.#
#.
1 1 2 2
~~~

#### 예제 출력 3
~~~
-1
~~~

[출처](https://www.acmicpc.net/problem/16930)

---

### 문제풀이



---

#### 나의 풀이

~~~python
from collections import deque
import sys

n, m, k = map(int, input().split())
gym = [sys.stdin.readline().rstrip() for _ in range(n)]
gym = [[0 if xy == "." else -1 for xy in g] for g in gym]
x1, y1, x2, y2 = map(int, input().split())
x1, y1, x2, y2 = x1 - 1, y1 - 1, x2 - 1, y2 - 1 
que = deque([(x1, y1)])

def bfs():
    while que:
        x, y = que.popleft()

        for w1, w2 in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
            for i in range(1, k + 1):
                dx, dy = x + (w1 * i), y + (w2 * i)
                if dx < 0 or dx >= n or dy < 0 or dy >= m or gym[dx][dy] == -1:
                    break
                if dx == x2 and dy == y2:
                    return gym[x][y] + 1
                if gym[dx][dy] and gym[dx][dy] <= gym[x][y]:
                    break
                if not gym[dx][dy]:
                    que.append((dx, dy))
                    gym[dx][dy] = gym[x][y] + 1
    return -1

print(bfs())
~~~
