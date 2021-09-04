# 특정한 최단 경로 (Gold 4)

### 문제 설명

방향성이 없는 그래프가 주어진다. 세준이는 1번 정점에서 N번 정점으로 최단 거리로 이동하려고 한다. 또한 세준이는 두 가지 조건을 만족하면서 이동하는 특정한 최단 경로를 구하고 싶은데, 그것은 바로 임의로 주어진 두 정점은 반드시 통과해야 한다는 것이다.   

세준이는 한번 이동했던 정점은 물론, 한번 이동했던 간선도 다시 이동할 수 있다. 하지만 반드시 최단 경로로 이동해야 한다는 사실에 주의하라. 1번 정점에서 N번 정점으로 이동할 때, 주어진 두 정점을 반드시 거치면서 최단 경로로 이동하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 정점의 개수 N과 간선의 개수 E가 주어진다. (2 ≤ N ≤ 800, 0 ≤ E ≤ 200,000) 둘째 줄부터 E개의 줄에 걸쳐서 세 개의 정수 a, b, c가 주어지는데, a번 정점에서 b번 정점까지 양방향 길이 존재하며, 그 거리가 c라는 뜻이다. (1 ≤ c ≤ 1,000) 다음 줄에는 반드시 거쳐야 하는 두 개의 서로 다른 정점 번호 v1과 v2가 주어진다. (v1 ≠ v2, v1 ≠ N, v2 ≠ 1)

---

#### 출력

첫째 줄에 두 개의 정점을 지나는 최단 경로의 길이를 출력한다. 그러한 경로가 없을 때에는 -1을 출력한다.

---

#### 예제 입력 1

~~~
4 6
1 2 3
2 3 3
3 4 1
1 3 5
2 4 5
1 4 4
2 3
~~~

#### 예제 출력 1

~~~
7
~~~

[출처](https://www.acmicpc.net/problem/1504)

---

### 문제풀이

이번 문제는 그래프에서 최단 거리를 모두 구하는 문제입니다.   

이 문제에서 특이한 점은 특정 노드 2군데를 모두 지나야 한다는 점입니다.   

문제를 해결할 수 있는 경우의 수는 2가지가 있습니다.   

1. 1 -> v1 -> v2 -> n
2. 1 -> v2 -> v1 -> n

이 문제를 해결하기 위해서 저는 다익스트라 알고리즘을 3번 사용해 문제를 풀었습니다.   

1에서 시작하는 경우, v1에서 시작하는 경우, v2에서 시작하는 경우 3가지를 모두 해보면 정답을 맞출 수 있습니다.   

---

#### 나의 풀이

~~~python
import sys, heapq

n, e = map(int, input().split())
graph = [[] for _ in range(n + 1)]
INF = int(10e9)

for _ in range(e):
    a, b, c = map(int, sys.stdin.readline().split())
    graph[a].append([b, c])
    graph[b].append([a, c])

v1, v2 = map(int, input().split())

def dijkstra(start):
    dist = [INF] * (n + 1)
    dist[start] = 0
    heap = [(0, start)]

    while heap:
        cur_dist, cur_node = heapq.heappop(heap)

        if dist[cur_node] < cur_dist:
            continue

        for node, d in graph[cur_node]:
            cost = dist[cur_node] + d
            if cost < dist[node]:
                dist[node] = cost
                heapq.heappush(heap, (cost, node))
    return dist

dist_1 = dijkstra(1)
dist_v1 = dijkstra(v1)
dist_v2 = dijkstra(v2)

m = min(dist_1[v1] + dist_v1[v2] + dist_v2[n], dist_1[v2] + dist_v2[v1] + dist_v1[n])

print(m if m < INF else -1)
~~~
