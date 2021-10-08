# 바이러스 (Silver 3)

### 문제

신종 바이러스인 웜 바이러스는 네트워크를 통해 전파된다. 한 컴퓨터가 웜 바이러스에 걸리면 그 컴퓨터와 네트워크 상에서 연결되어 있는 모든 컴퓨터는 웜 바이러스에 걸리게 된다.

예를 들어 7대의 컴퓨터가 <그림 1>과 같이 네트워크 상에서 연결되어 있다고 하자. 1번 컴퓨터가 웜 바이러스에 걸리면 웜 바이러스는 2번과 5번 컴퓨터를 거쳐 3번과 6번 컴퓨터까지 전파되어 2, 3, 5, 6 네 대의 컴퓨터는 웜 바이러스에 걸리게 된다. 하지만 4번과 7번 컴퓨터는 1번 컴퓨터와 네트워크상에서 연결되어 있지 않기 때문에 영향을 받지 않는다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/zmMEZZ8ioN6rhCdHmcIT4a7.png">

어느 날 1번 컴퓨터가 웜 바이러스에 걸렸다. 컴퓨터의 수와 네트워크 상에서 서로 연결되어 있는 정보가 주어질 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 출력하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에는 컴퓨터의 수가 주어진다. 컴퓨터의 수는 100 이하이고 각 컴퓨터에는 1번 부터 차례대로 번호가 매겨진다. 둘째 줄에는 네트워크 상에서 직접 연결되어 있는 컴퓨터 쌍의 수가 주어진다. 이어서 그 수만큼 한 줄에 한 쌍씩 네트워크 상에서 직접 연결되어 있는 컴퓨터의 번호 쌍이 주어진다.

---

#### 출력

1번 컴퓨터가 웜 바이러스에 걸렸을 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 첫째 줄에 출력한다.

---

#### 예제 입력 1
~~~
7
6
1 2
2 3
1 5
5 2
5 6
4 7
~~~

#### 예제 출력 1
~~~
4
~~~

출처 : https://www.acmicpc.net/problem/2606

---

### 문제풀이

이번 문제는 연결된 컴포넌트의 개수를 세는 문제입니다.

1번 컴퓨터와 연결된 컴포넌트의 개수를 출력하면 정답을 맞출 수 있습니다.

저는 dfs를 이용해 문제를 풀었습니다.

---

#### 나의 풀이

~~~python
def solution(graph, computers):
    # 컴퓨터의 수만큼 방문 리스트를 초기화
    visited = [False] * (computers + 1)
    # 1번 컴퓨터와 연결된 노드를 dfs 로 탐색
    dfs(graph, 1, visited)
    # 1번 컴퓨터를 통해 바이러스에 감염된 컴퓨터의 숫자를 반환
    return visited.count(True) - 1


def dfs(graph, node, visited):
    # 이미 방문한 노드는 확인하지 않음
    if visited[node]:
        return False
    # 현재 노드를 방문 처리
    visited[node] = True
    # 현재 노드와 인접한 노드를 모두 dfs 로 탐색
    for i in graph[node]:
        # 방문하지 않았던 노드만 탐색
        if not visited[i]:
            dfs(graph, i, visited)


def create_graph(computers, pairs):
    # graph 를 초기화, 인덱스를 맞춰주기 위해 컴퓨터의 숫자보다 1 크게 설정
    graph = [[] for _ in range(computers + 1)]

    for i in range(pairs):
        a, b = map(int, input().split())
        graph[a].append(b)
        graph[b].append(a)

    return graph


if __name__ == "__main__":
    n = int(input())
    p = int(input())
    print(solution(create_graph(n, p), n))
~~~
