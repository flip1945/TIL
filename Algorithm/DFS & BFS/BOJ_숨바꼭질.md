# 숨바꼭질

### 문제 설명

수빈이는 동생과 숨바꼭질을 하고 있다. 수빈이는 현재 점 N(0 ≤ N ≤ 100,000)에 있고, 동생은 점 K(0 ≤ K ≤ 100,000)에 있다.   

수빈이는 걷거나 순간이동을 할 수 있다.   

만약, 수빈이의 위치가 X일 때 걷는다면 1초 후에 X-1 또는 X+1로 이동하게 된다.   

순간이동을 하는 경우에는 1초 후에 2\*X의 위치로 이동하게 된다.   

수빈이와 동생의 위치가 주어졌을 때, 수빈이가 동생을 찾을 수 있는 가장 빠른 시간이 몇 초 후인지 구하는 프로그램을 작성하시오.   

---

#### 입력

첫 번째 줄에 수빈이가 있는 위치 N과 동생이 있는 위치 K가 주어진다. N과 K는 정수이다.

---

#### 출력

수빈이가 동생을 찾는 가장 빠른 시간을 출력한다.   

| 예제 입력 | 예제 출력 |
|----------|----------|
|   5 17   |      4   |

[출처](https://www.acmicpc.net/problem/1697)

---

### 문제풀이

최단 시간, 최단 거리 등을 구하는 문제는 BFS를 적용해 해결할 수 있습니다.   

BFS를 이용해 목적지까지의 최단 거리를 완전 탐색합니다.   

수빈이가 할 수 있는 선택은 총 3가지로, -1, +1, \*2입니다.   

모든 위치에서 3가지 선택을 하면서 목적지에 도달하면 종료합니다.   

모든 선택지를 확인하면서 가기 때문에 목적지에 도달했을 때, 최단 시간이 반환됩니다.   

현재 위치가 0 이하 100,000 이상이 되는 경우는 탐색하지 않습니다.      

~~~python
from collections import deque

def solution(n, k):
    answer = bfs(n, k)
    return answer

def bfs(start, target):
    # 방문 정보를 초기화
    visited = [False] * 100001
    time = 0
    que = deque([start])

    while que:
        # 현재 큐에 남아 있는 수만큼 실행
        for i in range(len(que)):
            v = que.popleft()
            # 이미 방문한 노드라면 생략
            if visited[v]:
                continue
            # 현재 노드가 원하는 위치라면 지금까지 걸린 시간을 반환
            if v == target:
                return time
            # 현재 노드를 방문처리
            visited[v] = True
            # 매 노드에서 3가지 선택지를 확인
            for j in [v - 1, v + 1, v * 2]:
                # 확인해야 하는 노드가 탐색 범위 이내인지 확인
                if 0 <= j <= 100000:
                    # 이미 방문한 노드는 큐에 추가하지 않음
                    if not visited[j]:
                        # 방문하지 않은 노드를 탐색하기 위해 큐에 추가
                        que.append(j)
        # 걸리는 시간을 1 추가
        time += 1

if __name__ == "__main__":
    a, b = map(int, input().split())
    print(solution(a, b))
~~~

#### 다른 사람의 풀이

모든 위치에 도달하는 정보를 저장하는 리스트를 만드는 방법을 적용한 풀이입니다.

~~~python
from collections import deque
N,K=map(int,input().split())
Max=10**5
D=[-1]*(Max+1)
D[N]=0
q=deque()
q.append(N)
while q:
    x=q.popleft()
    for nx in [x-1,x+1,2*x]:
        if 0<=nx<=Max and D[nx]==-1:
            q.append(nx)
            D[nx]=D[x]+1
print(D[K])
~~~

분할 정복과 재귀 호출을 통한 풀이입니다.

~~~python
def solution(n,k):
    if n>=k:
        return n-k
    elif k == 1:
        return 1
    elif k%2:
        return 1+min(solution(n,k-1),solution(n,k+1))
    else:
        return min(k-n, 1+solution(n,k//2))
    
n, k = map(int,input().split())
print(solution(n,k))
~~~
