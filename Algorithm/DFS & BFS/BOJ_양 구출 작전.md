# 양 구출 작전 (Gold 2)

### 문제

N개의 섬으로 이루어진 나라가 있습니다. 섬들은 1번 섬부터 N번 섬까지 있습니다.   

1번 섬에는 구명보트만 있고 다른 섬에는 양들 또는 늑대들이 살고 있습니다.   

늘어나는 늑대의 개체 수를 감당할 수 없던 양들은 구명보트를 타고 늑대가 없는 나라로 이주하기로 했습니다.   

각 섬에서 1번 섬으로 가는 경로는 유일하며 i번 섬에는 pi번 섬으로 가는 다리가 있습니다.    

양들은 1번 섬으로 가는 경로로 이동하며 늑대들은 원래 있는 섬에서 움직이지 않고 섬으로 들어온 양들을 잡아먹습니다. 늑대는 날렵하기 때문에 섬에 들어온 양을 항상 잡을 수 있습니다. 그리고 늑대 한 마리는 최대 한 마리의 양만 잡아먹습니다.   

얼마나 많은 양이 1번 섬에 도달할 수 있을까요?   

---

#### 입력

첫 번째 줄에 섬의 개수 N (2 ≤ N ≤ 123,456) 이 주어집니다.   

두 번째 줄부터 N-1개에 줄에 2번 섬부터 N번 섬까지 섬의 정보를 나타내는 ti, ai, pi (1 ≤ ai ≤ 109, 1 ≤ pi ≤ N) 가 주어집니다.   

ti가 'W' 인 경우 i번 섬에 늑대가 ai마리가 살고 있음을, ti가 'S'인 경우 i번 섬에 양이 ai마리가 살고 있음을 의미합니다. pi는 i번째 섬에서 pi번 섬으로 갈 수 있는 다리가 있음을 의미합니다.   

---

#### 출력

첫 번째 줄에 구출할 수 있는 양의 수를 출력합니다.

---

#### 예제 입력 1
~~~
4
S 100 3
W 50 1
S 10 1
~~~

#### 예제 출력 1
~~~
60
~~~

#### 예제 입력 2
~~~
7
S 100 1
S 100 1
W 100 1
S 1000 2
W 1000 2
S 900 6
~~~

#### 예제 출력 2
~~~
1200
~~~

출처 : https://www.acmicpc.net/problem/16437

---

### 문제풀이

이번 문제는 트리 구조로 이루어진 자료구조에서 dfs를 사용하는 문제입니다.   

dfs만 이용해서 푸는 문제는 오랜만이어서 재밌게 풀었습니다.   

이 문제의 핵심은 양의 숫자 계산을 언제 해야 하는 지 정하는 것입니다.   

dfs 탐색을 하고 더 이상 탐색할 수 없으면 마지막 노드까지 간 것이므로 그 때부터 양의 숫자 계산을 해주면 됩니다.   

그렇게 하면 제일 아래쪽의 섬부터 양의 숫자를 연산을 할 수 있게됩니다.   

트리 구조 순회에서 후위 순회랑 비슷한 형태로 연산을 해주면 됩니다.   

이 문제를 처음 풀었을 때, 시간 초과를 해서 로직이 잘못된 줄 알았습니다.   

그런데 다시 생각해보니 로직이 잘못된 것 같지는 않았습니다.   

혹시 재귀를 사용해서 느린 것인가 추측하고 있었는데,   

정확한 이유를 찾아보니 input()을 할 때 시간이 오래 걸리는 것이었습니다.

그래서 input() 대신 sys.stdin.readline()을 사용하니 바로 통과할 수 있었습니다.   

---

#### 나의 풀이

~~~python
import sys
sys.setrecursionlimit(10**6)
n = int(input())
# 섬의 연결 정보를 저장할 리스트
graph = [[] for _ in range(n+1)]
# 동물 정보를 저장할 리스트
animal = [[] for _ in range(n+1)]
# 양과 늑대가 있는 모든 노드를 입력
for i in range(2, n+1):
    a, b, c = sys.stdin.readline().split()
    b, c = int(b), int(c)
    graph[c].append(i)
    animal[i] = [a, b]

def dfs(node):
    sheep = 0

    for v in graph[node]:
        sheep += dfs(v)
    # dfs 탐색 후 더 이상 탐색할 노드가 없다면 양의 숫자를 연산
    sheep += animal[node][1] if animal[node][0] == "S" else -animal[node][1]
    return max(sheep, 0)

animal[1] = [0, 0]
print(dfs(1))
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/12845996

~~~python
import sys
sys.setrecursionlimit(10**9)
input = sys.stdin.readline
N = int(input())
S = [0]*N
W = [0]*N
tree = [[] for _ in range(N)]
for i in range(1,N):
    t,a,p = input().split()
    if t == 'S': S[i] = int(a)
    else: W[i] = int(a)
    tree[int(p)-1].append(i)
def count(p):
    ret = S[p]
    for u in tree[p]:
        ret += count(u)
    ret = max(0, ret-W[p])
    return ret
print(count(0))
~~~
