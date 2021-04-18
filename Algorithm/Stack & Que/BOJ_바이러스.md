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
