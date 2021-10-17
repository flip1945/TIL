# 가장 먼 노드(Level 3)

### 문제 설명

n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.   

노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.   

---

#### 제한사항

* 노드의 개수 n은 2 이상 20,000 이하입니다.

* 간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.

* vertex 배열 각 행 \[a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.

---

#### 입출력 예

|n	|vertex|	return|
|-|-|-|
|6	|\[\[3, 6], \[4, 3], \[3, 2], \[1, 3], \[1, 2], \[2, 4], \[5, 2]]|	3|

#### 입출력 예 설명

예제의 그래프를 표현하면 아래 그림과 같고, 1번 노드에서 가장 멀리 떨어진 노드는 4,5,6번 노드입니다.

<img src = "https://grepp-programmers.s3.amazonaws.com/files/ybm/fadbae38bb/dec85ab5-0273-47b3-ba73-fc0b5f6be28a.png">

출처 : https://programmers.co.kr/learn/courses/30/lessons/17687

---

### 문제풀이

이 문제는 그래프 문제입니다.   

주어진 2차원 배열을 그래프로 형태로 만들어서 가장 먼 노드를 알아내면 됩니다.   

가장 먼 노드를 알 때는 bfs를 사용해서 모든 노드의 거리를 파악하면 편합니다.   

푸는 방법은 다음과 같습니다.   

1. 먼저 bfs로 모든 모드까지의 거리를 저장하는 리스트를 만듭니다.   

2. 저장된 리스트에서 가장 먼 노드의 거리를 확인합니다.   

3. 가장 먼 노드의 거리와 같은 노드가 몇 개 있는지 확인해서 반환합니다.   

---

#### 나의 풀이

~~~python
from collections import deque

def solution(n, edge):
    # 그래프 리스트 초기화
    graph = [[] for i in range(n+1)]
    # 방문 리스트 초기화
    visited = [False for i in range(n+1)]
    # 노드의 거리를 저장할 리스트 초기화
    result = [0] * (n+1)
    # 그래프 생성
    for start, end in edge:
        graph[start].append(end)
        graph[end].append(start)
    # 1번 노드에서 bfs 실행
    answer = bfs(graph, 1, visited, result)
    answer.sort()
    # 가장 거리가 먼 노드의 값과 같은 노드들을 카운트 해서 반환
    return answer.count(answer[-1])

def bfs(graph, start, visited, result):
    # bfs에 사용할 que를 초기화
    que = deque([(start, 0)])
    # que가 있는 동안 반복
    while que:
        # 현재 노드와 이전 노드를 초기화
        node, pre_node = que.popleft()
        # 현재 노드가 방문한 노드라면 생략
        if visited[node]:
            continue
        # 현재 노드를 방문처리
        visited[node] = True
        # 현재 노드와 연결된 노드들을 모두 확인
        for i in graph[node]:
            # que에 다음에 확인할 노드를 push
            que.append([i, node])
        # 결과 리스트에 현재 노드까지의 거리를 저장
        result[node] = result[pre_node] + 1
    
    return result
~~~

---

#### 다른 사람의 풀이

출처 : https://programmers.co.kr/learn/courses/30/lessons/17687/solution_groups?language=python3

~~~pyhton
def solution(n, edge):
    graph =[  [] for _ in range(n + 1) ]
    distances = [ 0 for _ in range(n) ]
    is_visit = [False for _ in range(n)]
    queue = [0]
    is_visit[0] = True
    for (a, b) in edge:
        graph[a-1].append(b-1)
        graph[b-1].append(a-1)

    while queue:
        i = queue.pop(0)

        for j in graph[i]:
            if is_visit[j] == False:
                is_visit[j] = True
                queue.append(j)
                distances[j] = distances[i] + 1

    distances.sort(reverse=True)
    answer = distances.count(distances[0])

    return answer
~~~
