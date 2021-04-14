# Algorithm   

알고리즘 공부입니다.  

TDD 방식을 적용해 알고리즘 공부합니다.

## 공부할 목록

* Greedy
* DFS & BFS
* Sort
* Binary Search
* Dinamic Programming
* Graph

## 예제 소스 코드

### dfs 알고리즘

~~~python
def dfs(graph, v, visited):
    # 현재 노드 방문 처리
    visited[v] = True
    print(v, end=' ')
    # 현재 노드와 연결된 다른 노드를 재귀적으로 방문
    for i in graph[v]:
        if not visited[i]:
           dfs(graph, i, visited)
           
    graph = [[]] # 생략
    visited = [False] * 9
    dfs(graph, 1, visited)
~~~

### bfs 알고리즘

~~~python
from collections import deque

def bfs(graph, start, visited):
    queue = deque([start])
    # 현재 노드 방문 처리
    visited[start] = True
    # 큐가 빌 때까지 반복
    while queue:
        # 큐에서 원소 하나를 빼서 출력
        v = queue.popleft()
        print(v, end=' ')
        # 아직 방문하지 않은 인접한 원소를 모두 큐에 삽입
        for i in graph[v]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True
           
    graph = [[]] # 생략
    visited = [False] * 9
    bfs(graph, 1, visited)
~~~
