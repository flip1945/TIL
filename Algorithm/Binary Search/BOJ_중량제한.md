# 중량제한 (Gold 4)

### 문제

N(2 ≤ N ≤ 10,000)개의 섬으로 이루어진 나라가 있다. 이들 중 몇 개의 섬 사이에는 다리가 설치되어 있어서 차들이 다닐 수 있다.   

영식 중공업에서는 두 개의 섬에 공장을 세워 두고 물품을 생산하는 일을 하고 있다. 물품을 생산하다 보면 공장에서 다른 공장으로 생산 중이던 물품을 수송해야 할 일이 생기곤 한다. 그런데 각각의 다리마다 중량제한이 있기 때문에 무턱대고 물품을 옮길 순 없다. 만약 중량제한을 초과하는 양의 물품이 다리를 지나게 되면 다리가 무너지게 된다.   

한 번의 이동에서 옮길 수 있는 물품들의 중량의 최댓값을 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 N, M(1 ≤ M ≤ 100,000)이 주어진다. 다음 M개의 줄에는 다리에 대한 정보를 나타내는 세 정수 A, B(1 ≤ A, B ≤ N), C(1 ≤ C ≤ 1,000,000,000)가 주어진다. 이는 A번 섬과 B번 섬 사이에 중량제한이 C인 다리가 존재한다는 의미이다. 서로 같은 두 섬 사이에 여러 개의 다리가 있을 수도 있으며, 모든 다리는 양방향이다. 마지막 줄에는 공장이 위치해 있는 섬의 번호를 나타내는 서로 다른 두 정수가 주어진다. 공장이 있는 두 섬을 연결하는 경로는 항상 존재하는 데이터만 입력으로 주어진다.

---

#### 출력

첫째 줄에 답을 출력한다.

---

#### 예제 입력 1
~~~
3 3
1 2 2
3 1 3
2 3 2
1 3
~~~

#### 예제 출력 1
~~~
3
~~~

출처 : https://www.acmicpc.net/problem/1939

---

### 문제풀이

이번 문제는 이분 탐색 + 그래프 탐색 문제입니다.   

특정 중량이 출발지에서 목적지까지 도착할 수 있는지 확인하는 문제입니다.   

저는 처음에 문제를 풀 때, 한 번 탐색할 때 바로 문제를 해결하는 방법을 생각했습니다.   

다익스트라 알고리즘을 응용해서 문제를 푸는 방식을 생각했는데, 더 쉽게 푸는 방법이 있을 것 같아서 다른 방법을 찾아봤습니다.   

찾아본 결과 이분 탐색을 이용해 문제를 해결하는 방법이 있다는 걸 알게됐습니다.   

이분 탐색으로 확인할 중량을 탐색하고, bfs를 실행합니다.   

이분 탐색을 이용하면 확인하는 경우가 O(logN)으로 엄청 줄기 때문에 충분히 문제를 해결할 수 있습니다.   

저는 중량을 정하고 그래프 탐색을 하면 시간 복잡도가 많이 증가할 거라고 생각해서 위와 같은 방식은 생각하지 못했습니다.   

그런데 다시 생각해보니 이분 탐색을 이용하면 탐색 횟수가 많지 않기 때문에 중량을 정하는 것을 이분 탐색으로 하면

시간 복잡도가 별로 증가하지 않아서 문제를 쉽게 해결할 수 있었습니다.

---

#### 나의 풀이

~~~python
import sys
from collections import deque
n, m = map(int, input().split())
graph = [[] for _ in range(n + 1)]

for _ in range(m):
    a, b, c = map(int, sys.stdin.readline().split())
    graph[a].append([c, b])
    graph[b].append([c, a])

start, end = map(int, input().split())

def bfs(limit):
    visited = [False] * (n + 1)
    queue = deque([(0, start)])

    while queue:
        weight, cur_node = queue.popleft()

        for next_weight, next_node in graph[cur_node]:
            if not visited[next_node] and limit <= next_weight:
                if next_node == end:
                    return True
                queue.append((next_weight, next_node))
                visited[next_node] = True

    return False

left, right = 1, 1000000000
# 이분 탐색으로 최대 중량을 탐색함
while left <= right:
    mid = left + (right - left) // 2
    # bfs를 통해 해당 중량이 목적지에 도달할 수 있는지 확인함
    if bfs(mid):
        left = mid + 1
    else:
        right = mid - 1

print(right)
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/19261362

~~~python
from collections import defaultdict as dd
import heapq as h
import sys
input=sys.stdin.readline
n,m=map(int,input().split())
e=[dd(int) for _ in range(n)]
for _ in range(m):
    a,b,c=map(int,input().split())
    e[a-1][b-1]+=c
    e[b-1][a-1]+=c
s,t=map(int,input().split())
d=[0]*n
d[s-1]=-10**9
q=[(-10**9,s-1)]
while q:
    w,a=h.heappop(q)
    if w>d[a]: continue
    if a==t-1: break
    for b in e[a]:
        if d[b]>max(w,-e[a][b]):
        d[b]=max(w,-e[a][b])
        h.heappush(q,(d[b],b))
print(-d[t-1])
~~~
