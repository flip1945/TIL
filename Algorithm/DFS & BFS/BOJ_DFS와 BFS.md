# DFS와 BFS (Silver 2)

### 문제

그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 N번까지이다.    

---

#### 입력

첫째 줄에 정점의 개수 N(1 ≤ N ≤ 1,000), 간선의 개수 M(1 ≤ M ≤ 10,000), 탐색을 시작할 정점의 번호 V가 주어진다. 다음 M개의 줄에는 간선이 연결하는 두 정점의 번호가 주어진다. 어떤 두 정점 사이에 여러 개의 간선이 있을 수 있다. 입력으로 주어지는 간선은 양방향이다.   

---

#### 출력

첫째 줄에 DFS를 수행한 결과를, 그 다음 줄에는 BFS를 수행한 결과를 출력한다. V부터 방문된 점을 순서대로 출력하면 된다.   

---

#### 예제 입력 1
~~~
4 5 1   
1 2   
1 3   
1 4   
2 4   
3 4   
~~~

#### 예제 출력 1
~~~
1 2 4 3
1 2 3 4
~~~

#### 예제 입력 2
~~~
5 5 3
5 4
5 2
1 2
3 4
3 1 
~~~

#### 예제 출력 2
~~~
3 1 2 5 4
3 1 4 2 5
~~~

#### 예제 입력 3
~~~
1000 1 1000
999 1000
~~~

#### 예제 출력 3
~~~
1000 999
1000 999
~~~

출처 : https://www.acmicpc.net/problem/1260

---

### 문제풀이

이 문제는 dfs와 bfs를 모두 사용해서 풀어야 하는 문제입니다.   

기본적인 문제기 때문에 간단한 dfs, bfs 함수를 만들어서 풀었습니다.   

조금 고민했던 부분은 그래프를 정렬하는 부분입니다.   

처음에는 완성된 그래프를 완성하기 전에 입력된 값을 정렬시켰는데, 그렇게 하면 제대로 정렬되지 않는 것을 깨달았습니다.   

그래서 for문을 돌려서 각 노드의 그래프를 정렬시켰습니다.   

---

#### 나의 풀이

~~~python
from collections import deque


def solution():
    n, m, v = map(int, input().split())
    graph = create_graph(n, m)
    graph = [sorted(g) for g in graph]
    result = [[], []]

    visited = [False for _ in range(n+1)]
    result[0] = dfs(graph, visited, v, [])

    visited = [False for _ in range(n+1)]
    result[1] = bfs(graph, visited, v, [])

    print(*result[0])
    print(*result[1])

# 그래프 생성 함수
def create_graph(n, m):
    edges = [list(map(int, input().split())) for i in range(m)]
    graph = [[] for i in range(n + 1)]
    # 간선의 방향이 양방향이기 때문에 양쪽 노드에서 그래프를 생성
    for edge in edges:
        graph[edge[0]].append(edge[1])
        graph[edge[1]].append(edge[0])

    return graph


def bfs(graph, visited, v, result):
    que = deque([v])

    while que:
        node = que.popleft()
        if visited[node]:
            continue
        visited[node] = True
        result.append(node)

        for i in graph[node]:
            que.append(i)

    return result


def dfs(graph, visited, v, result):
    if visited[v]:
        return

    visited[v] = True
    result.append(v)

    for i in graph[v]:
        dfs(graph, visited, i, result)

    return result


if __name__ == "__main__":
    solution()

~~~
