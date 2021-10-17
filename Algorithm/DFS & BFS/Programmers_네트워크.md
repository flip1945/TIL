# 네트워크 (Level 3)

### 문제 설명

네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

---

#### 제한사항

* 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.

* 각 컴퓨터는 0부터 n-1인 정수로 표현합니다.

* i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.

* computer[i][i]는 항상 1입니다.

---

#### 입출력 예

|n|	computers|	return|
|-|-|-|
|3|	\[\[1, 1, 0], \[1, 1, 0], \[0, 0, 1]]|	2|
|3|	\[\[1, 1, 0], \[1, 1, 1], \[0, 1, 1]]|	1|

#### 입출력 예 설명

##### 예제 #1

아래와 같이 2개의 네트워크가 있습니다.

<img src = "https://grepp-programmers.s3.amazonaws.com/files/ybm/5b61d6ca97/cc1e7816-b6d7-4649-98e0-e95ea2007fd7.png">

##### 예제 #2

아래와 같이 1개의 네트워크가 있습니다.

<img src = "https://grepp-programmers.s3.amazonaws.com/files/ybm/7554746da2/edb61632-59f4-4799-9154-de9ca98c9e55.png">

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
