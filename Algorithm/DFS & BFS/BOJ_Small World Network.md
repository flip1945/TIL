# Small World Network (Silver 1)

### 문제

작은 세상 네트워크(Small World Network)란 Milgram 교수가 1967년에 처음으로 밝혀낸 이론이다.

간단히 설명하자면 전체 네트워크가 거대하더라도 전체가 서로 가깝게 연결될 수 있다는 이론이다.

해당 이론에서 Milgram 교수는 지구에 있는 모든 사람들이 최대 6단계로 연결될 수 있다고 주장하였다.

예를 들어 이 문제를 만든 김 모 씨(23)와 이지은님(27)이 서로 생판 모르는 관계라도 최대 6단계만 거치면 서로 연결이 되어있다는 것이다.

<img src="https://upload.acmicpc.net/1033b3fc-4c88-4483-8bc3-88836630b1cd/-/preview/" width="600px" height="215px">

위의 그림에서 정점은 사람, 간선은 친구 관계라 할 때 왼쪽 그래프의 모든 정점들은 서로 최소 6단계 이하로 연결되어 있으므로 작은 세상 네트워크를 만족한다. 그러나 오른쪽 그래프의 초록색 정점끼리는 최소 7단계를 거쳐서 연결되어 있으므로 작은 세상 네트워크를 만족하지 않는다. 

이 이론에 대해 의구심이 생긴 김 모 씨는 정말 최대 6단계만 거치면 지구상의 모든 사람들이 서로 연결이 될 수 있는지 확인하고 싶었다.

김 모 씨를 위해 지구상의 모든 사람들의 친구 관계가 주어졌을 때 작은 세상 네트워크가 실제로 만족하는지 확인하는 프로그램을 만들어보자.

---

#### 입력

첫 번째 줄에 지구에 있는 사람의 수 N과 친구 관계의 개수 K가 주어진다. 모든 사람은 1부터 N까지 번호가 매겨져 있다. (1 ≤ N ≤ 100, 0 ≤ K ≤ N×(N-1)/2)

두 번째 줄부터 K+1번째 줄까지 친구 관계를 나타내는 A B가 한 줄에 하나씩 주어진다. (1 ≤ A, B ≤ N)

A와 B가 친구면 B와 A도 친구다. 자기 자신과 친구인 경우는 없다. A와 B의 친구 관계는 중복되어 입력되지 않는다.

---

#### 출력

해당 네트워크가 작은 세상 네트워크를 만족하면 "Small World!"를, 만족하지 않는다면 "Big World!"를 출력한다.

출처 : https://www.acmicpc.net/problem/18243

---

### 문제풀이

이번 문제는 bfs로 풀 수 있는 문제입니다.

전체 노드의 크기가 100으로 작기 때문에 모든 노드에서 bfs를 실행하면 6번 이내로 연결 가능한지 쉽게 알 수 있습니다.

저는 bfs + 완전 탐색으로 문제를 풀었지만 플로이드 워셜 알고리즘을 이용해 문제를 해결할 수도 있습니다.

---

#### 나의 풀이

~~~python
from collections import deque
n, k = map(int, input().split())
graph = [[] for _ in range(n + 1)]
for _ in range(k):
    a, b = map(int, input().split())
    graph[a].append(b)
    graph[b].append(a)


def bfs(start):
    queue = deque([start])
    visited = [-1] * (n + 1)
    visited[start] = 0

    while queue:
        current_node = queue.popleft()

        if visited[current_node] > 6:
            return False

        for next_node in graph[current_node]:
            if visited[next_node] == -1:
                queue.append(next_node)
                visited[next_node] = visited[current_node] + 1

    return False if -1 in visited[1:] else True


for i in range(1, n + 1):
    if not bfs(i):
        print("Big World!")
        break
else:
    print("Small World!")

~~~

#### 다른 사람의 풀이

플로이드 워셜 알고리즘 풀이

https://www.acmicpc.net/source/16875092
