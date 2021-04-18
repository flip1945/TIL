~~~python
def solution(n, computers):
    answer = 0
    visited = [False] * n

    # 모든 노드를 확인
    for i in range(n):
        # 처음 방문한 네트워크라면 네트워크의 수(answer)를 증가시킴
        if dfs(computers, i, visited):
            answer += 1
    return answer


def dfs(graph, node, visited):
    # 방문한 네트워크인지 확인하고 방문했던 네트워크라면 False 를 리턴
    if visited[node]:
        return False
    # 현재 노드를 방문 처리
    visited[node] = True
    # 현재 노드에서 인접한 모든 노드를 탐색
    for i in range(len(graph[node])):
        # 아직 방문하지 않은 노드, 연결된 노드, 그리고 자기 자신 노드가 아닌지 확인하고 인접노드를 탐색
        if not visited[i] and graph[node][i] and i != node:
            dfs(graph, i, visited)
    return True


if __name__ == "__main__":
    print(solution(3, [[1, 1, 0], [1, 1, 0], [0, 0, 1]]))
    print(solution(3, [[1, 1, 0], [1, 1, 1], [0, 1, 1]]))
~~~
