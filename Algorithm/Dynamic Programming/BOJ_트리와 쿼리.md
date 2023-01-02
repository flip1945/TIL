# 트리와 쿼리 (Gold 5)

### 문제

간선에 가중치와 방향성이 없는 임의의 루트 있는 트리가 주어졌을 때, 아래의 쿼리에 답해보도록 하자.

* 정점 U를 루트로 하는 서브트리에 속한 정점의 수를 출력한다.

만약 이 문제를 해결하는 데에 어려움이 있다면, 하단의 힌트에 첨부한 문서를 참고하자.

---

#### 입력

트리의 정점의 수 N과 루트의 번호 R, 쿼리의 수 Q가 주어진다. (2 ≤ N ≤ 105, 1 ≤ R ≤ N, 1 ≤ Q ≤ 105)

이어 N-1줄에 걸쳐, U V의 형태로 트리에 속한 간선의 정보가 주어진다. (1 ≤ U, V ≤ N, U ≠ V)

이는 U와 V를 양 끝점으로 하는 간선이 트리에 속함을 의미한다.

이어 Q줄에 걸쳐, 문제에 설명한 U가 하나씩 주어진다. (1 ≤ U ≤ N)

입력으로 주어지는 트리는 항상 올바른 트리임이 보장된다.

---

#### 출력

Q줄에 걸쳐 각 쿼리의 답을 정수 하나로 출력한다.

출처 : https://www.acmicpc.net/problem/15681

---

### 문제풀이

이번 문제는 tree dp 문제입니다.

처음에 tree dp 문제인 줄은 모르고 dfs문제라고만 생각하고 문제를 풀었는데 풀고보니 tree dp 문제 유형이라는 걸 깨달았습니다.

처음 풀어보는 유형인데 생각보다 재밌는 문제였습니다.

시간초과가 계속 나서 뭘 잘못 풀었나 확인해봤는데 input() 시간이 오래걸려서 그런거라 그 부분만 고치고 빠르게 통과시킬 수 있었습니다.

---

#### 나의 풀이

~~~python
import sys
sys.setrecursionlimit(10**6)
input = sys.stdin.readline
n, r, q = map(int, input().split())
tree = [[] for _ in range(n + 1)]
size = [1] * (n + 1)

for _ in range(n - 1):
    u, v = map(int, input().split())
    tree[u].append(v)
    tree[v].append(u)

def dfs(current_node, parent_node):
    for next_node in tree[current_node]:
        if next_node != parent_node:
            dfs(next_node, current_node)
            size[current_node] += size[next_node]

dfs(r, None)

for _ in range(q):
    print(size[int(input())])
~~~
