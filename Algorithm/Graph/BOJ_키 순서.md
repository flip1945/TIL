# 키 순서 (Gold 4)

### 문제 설명

1번부터 N번까지 번호가 붙여져 있는 학생들에 대하여 두 학생끼리 키를 비교한 결과의 일부가 주어져 있다. 단, N명의 학생들의 키는 모두 다르다고 가정한다. 예를 들어, 6명의 학생들에 대하여 6번만 키를 비교하였고, 그 결과가 다음과 같다고 하자.   

* 1번 학생의 키 < 5번 학생의 키

* 3번 학생의 키 < 4번 학생의 키

* 5번 학생의 키 < 4번 학생의 키

* 4번 학생의 키 < 2번 학생의 키

* 4번 학생의 키 < 6번 학생의 키

* 5번 학생의 키 < 2번 학생의 키

이 비교 결과로부터 모든 학생 중에서 키가 가장 작은 학생부터 자신이 몇 번째인지 알 수 있는 학생들도 있고 그렇지 못한 학생들도 있다는 사실을 아래처럼 그림을 그려 쉽게 확인할 수 있다. a번 학생의 키가 b번 학생의 키보다 작다면, a에서 b로 화살표를 그려서 표현하였다.   

<img src="https://upload.acmicpc.net/8f9e2484-a3aa-4b97-b1fa-387df4ae58d0/-/preview/" width=100>

1번은 5번보다 키가 작고, 5번은 4번보다 작기 때문에, 1번은 4번보다 작게 된다. 그러면 1번, 3번, 5번은 모두 4번보다 작게 된다. 또한 4번은 2번과 6번보다 작기 때문에, 4번 학생은 자기보다 작은 학생이 3명이 있고, 자기보다 큰 학생이 2명이 있게 되어 자신의 키가 몇 번째인지 정확히 알 수 있다. 그러나 4번을 제외한 학생들은 자신의 키가 몇 번째인지 알 수 없다.   

학생들의 키를 비교한 결과가 주어질 때, 자신의 키가 몇 번째인지 알 수 있는 학생들이 모두 몇 명인지 계산하여 출력하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 학생들의 수 N (2 ≤ N ≤ 500)과 두 학생 키를 비교한 횟수 M (0 ≤ M ≤ N(N-1)/2)이 주어진다. 다음 M개의 각 줄에는 두 학생의 키를 비교한 결과를 나타내는 두 양의 정수 a와 b가 주어진다. 이는 번호가 a인 학생이 번호가 b인 학생보다 키가 작은 것을 의미한다.

---

#### 출력

자신이 키가 몇 번째인지 알 수 있는 학생이 모두 몇 명인지를 출력한다. 

---

#### 예제 입력 1

~~~
6 6
1 5
3 4
5 4
4 2
4 6
5 2
~~~

#### 예제 출력 1

~~~
1
~~~

#### 예제 입력 2

~~~
6 7
1 3
1 5
3 4
5 4
4 2
4 6
5 2
~~~

#### 예제 출력 2

~~~
2
~~~

#### 예제 입력 3

~~~
6 3
1 2
2 3
4 5
~~~

#### 예제 출력 3

~~~
0
~~~

[출처](https://www.acmicpc.net/problem/2458)

---

### 문제풀이

이 문제는 플로이드-와샬로 분류돼있는 문제입니다.   

키의 순서를 나타내는 정보들이 주어지는 데, 자신의 순위를 알고 있는 학생 수가 총 얼마인지 파악하는 문제입니다.   

먼저 방향 그래프를 만들어서 학생들을 연결해주고, 플로이드-와샬 알고리즘을 이용합니다.   

이렇게 하면 각 학생별로 자신 보다 키가 큰 학생들의 정보를 가지게됩니다.   

어떤 학생이 자신의 키 순위를 알고 있다는 것은 자신보다 키가 큰 학생들과 키가 작은 학생들을 모두 알고있어야 한다는 의미입니다.   

키가 큰 학생은 앞선 알고리즘의 결과로 알고 있으니, 자신보다 키가 작은 학생들이 누구인지 파악해야 합니다.   

자신보다 키가 작은 학생이 자신에 대한 정보를 가지고 있다면 자신보다 작다는 정보가됩니다.   

그래서 이를 확인해 주면 문제를 해결할 수 있습니다.   

저는 이 문제를 플로이드-와샬 알고리즘으로 풀었기 때문에 O(n^3)의 시간복잡도가 걸렸습니다.   

이 때문에 python3가 아닌 pypy로 문제를 제출했는데, 다른 분이 python3으로 문제를 해결하신 것을 보니 BFS로 해결할 수 있다는 것을 알게됐습니다.

---

#### 나의 풀이

~~~python
import sys
n, m = map(int, input().split())
INF = int(10e9)
graph = [[INF if i != j else 0 for j in range(n)] for i in range(n)]
# 그래프 생성
for _ in range(m):
    a, b = map(int, sys.stdin.readline().split())
    graph[a-1][b-1] = 1
# 플로이드-와샬 알고리즘 실행(키가 큰 학생들의 연결 정보 갱신)
for k in range(n):
    for i in range(n):
        for j in range(n):
            graph[i][j] = min(graph[i][j], graph[i][k] + graph[k][j])

answer = 0

for i in range(n):
    for j in range(n):
        # 상대방의 키 정보가 없을 때, 상대가 자신의 키 정보를 알고있다면
        if graph[i][j] == INF and graph[j][i] != INF:
            graph[i][j] = 1
        # 상대방의 키 정보가 없을 때, 상대가 자신의 키 정보를 모른다면
        elif graph[i][j] == INF:
            break
    else:
        answer += 1

print(answer)
~~~

---

#### 다른 사람의 풀이

1. 플로이드-와샬 알고리즘

~~~python
import sys
input = sys.stdin.readline

N, M = map(int, input().split())
arr = [[0] * (N) for _ in range(N)]
for _ in range(M):
    v, e = map(int, input().split())
    arr[v - 1][e - 1] = 1

# 플로이드-와샬 알고리즘, 임의의 숫자 i, j에 대해 연결되어 있는지를 검사한다.
for path in range(N):
    for i in range(N):
        for j in range(N):
            if arr[i][path] == 1 and arr[path][j] == 1:
                arr[i][j] =1
answer = 0
for i in range(N):
    check = 0
    for j in range(N):
        # i에서 j로 간다는 건 i가 j보다 작음
        # j에서 i로 온다는 건 j가 i보다 작음
        check += arr[i][j] + arr[j][i]
    # N - 1이라는 것은 자신을 제외한 모든 노드와 비교를 하였다는 것
    if check == N - 1:
        answer +=1
print(answer)
~~~

2. BFS 알고리즘

출처 : https://velog.io/@jinho0705/BOJ-2458.-%ED%82%A4-%EC%88%9C%EC%84%9Cpython

~~~python
import sys
from collections import deque

N,M = map(int,sys.stdin.readline().split())
graph = [[] for _ in range(N+1)]
cnt = [1]*(N+1)
q = deque([])

for _ in range(M):
  a,b = map(int,sys.stdin.readline().split())
  
  graph[a].append(b)

for i in range(1,N+1):
  
  visited = [False]*(N+1)

  q.append(i)
  visited[i] = True

  while q:

    j = q.popleft()

    if i!=j:
      cnt[i] += 1
      cnt[j] += 1

    for k in graph[j]:
    
      if not visited[k]:
        visited[k] = True

        q.append(k)

sys.stdout.write(str(cnt.count(N)))
~~~
