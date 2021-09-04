# 최단경로 (Gold 5)

### 문제 설명

방향그래프가 주어지면 주어진 시작점에서 다른 모든 정점으로의 최단 경로를 구하는 프로그램을 작성하시오. 단, 모든 간선의 가중치는 10 이하의 자연수이다.

---

#### 입력

첫째 줄에 정점의 개수 V와 간선의 개수 E가 주어진다. (1≤V≤20,000, 1≤E≤300,000) 모든 정점에는 1부터 V까지 번호가 매겨져 있다고 가정한다. 둘째 줄에는 시작 정점의 번호 K(1≤K≤V)가 주어진다. 셋째 줄부터 E개의 줄에 걸쳐 각 간선을 나타내는 세 개의 정수 (u, v, w)가 순서대로 주어진다. 이는 u에서 v로 가는 가중치 w인 간선이 존재한다는 뜻이다. u와 v는 서로 다르며 w는 10 이하의 자연수이다. 서로 다른 두 정점 사이에 여러 개의 간선이 존재할 수도 있음에 유의한다.

---

#### 출력

첫째 줄부터 V개의 줄에 걸쳐, i번째 줄에 i번 정점으로의 최단 경로의 경로값을 출력한다. 시작점 자신은 0으로 출력하고, 경로가 존재하지 않는 경우에는 INF를 출력하면 된다.

---

#### 예제 입력 1

~~~
5 6
1
5 1 1
1 2 2
1 3 3
2 3 4
2 4 5
3 4 6
~~~

#### 예제 출력 1

~~~
0
2
3
7
INF
~~~

[출처](https://www.acmicpc.net/problem/1753)

---

### 문제풀이

이번 문제는 그래프에서 최단 거리를 모두 구하는 문제입니다.   

각 노드별로 최단 거리를 구해야 하는데 저는 다익스트라 알고리즘을 이용해 문제를 풀었습니다.   

다익스트라 알고리즘은 O(VlogE) 시간에 모든 노드의 최단 거리를 구할 수 있는 알고리즘이기 때문에 이 문제를 푸는 데 사용할 수 있었습니다.   

BFS와 비슷한 방식으로 문제를 푸는데 queue가 아닌 heap을 이용해서 푼다는 차이점이 있습니다.   

---

#### 나의 풀이

~~~python
import sys, heapq

n, k = map(int, input().split())
start = int(input())

graph = [[] for _ in range(n + 1)]
INF = int(10e9)
dist = [INF] * (n + 1)

for i in range(k):
    u, v, w = map(int, sys.stdin.readline().split())
    graph[u].append((v, w))

heap = [(0, start)]
dist[start] = 0

while heap:
    w, v = heapq.heappop(heap)

    if dist[v] < w:
        continue

    for node, d in graph[v]:
        if dist[v] + d < dist[node]:
            dist[node] = dist[v] + d
            heapq.heappush(heap, (dist[v] + d, node))

print(*[d if d != INF else "INF" for d in dist[1:]], sep='\n')
~~~
