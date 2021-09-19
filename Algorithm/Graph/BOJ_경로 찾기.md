# 경로 찾기 (Silver 1)

### 문제 설명

가중치 없는 방향 그래프 G가 주어졌을 때, 모든 정점 (i, j)에 대해서, i에서 j로 가는 경로가 있는지 없는지 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 정점의 개수 N (1 ≤ N ≤ 100)이 주어진다. 둘째 줄부터 N개 줄에는 그래프의 인접 행렬이 주어진다. i번째 줄의 j번째 숫자가 1인 경우에는 i에서 j로 가는 간선이 존재한다는 뜻이고, 0인 경우는 없다는 뜻이다. i번째 줄의 i번째 숫자는 항상 0이다.

---

#### 출력

총 N개의 줄에 걸쳐서 문제의 정답을 인접행렬 형식으로 출력한다. 정점 i에서 j로 가는 경로가 있으면 i번째 줄의 j번째 숫자를 1로, 없으면 0으로 출력해야 한다.

---

#### 예제 입력 1

~~~
3
0 1 0
0 0 1
1 0 0
~~~

#### 예제 출력 1

~~~
1 1 1
1 1 1
1 1 1
~~~

#### 예제 입력 2

~~~
7
0 0 0 1 0 0 0
0 0 0 0 0 0 1
0 0 0 0 0 0 0
0 0 0 0 1 1 0
1 0 0 0 0 0 0
0 0 0 0 0 0 1
0 0 1 0 0 0 0
~~~

#### 예제 출력 2

~~~
1 0 1 1 1 1 1
0 0 1 0 0 0 1
0 0 0 0 0 0 0
1 0 1 1 1 1 1
1 0 1 1 1 1 1
0 0 1 0 0 0 1
0 0 1 0 0 0 0
~~~

[출처](https://www.acmicpc.net/problem/11403)

---

### 문제풀이

이번 문제는 그래프 연결요소를 파악하는 문제입니다.   

노드별로 서로가 연결되어 있는지 확인하는 문제로 n의 크기가 작기 때문에 플로이드 와샬 알고리즘으로 문제를 해결할 수 있습니다.   

저는 플로이드 와샬 알고리즘이 익숙하지 않아서 제대로 응용하지 못했는데, 다른 분들의 풀이를 보니 더 좋은 방법이 있었습니다.   

저는 연결 요소의 최단거리를 모두 구했지만 이 문제에서는 그럴필요까지는 없었습니다.   

노드끼리 연결돼있는지를 파악하면 되기 때문에 연결 유무만 파악하면 됩니다.   

그림을 그려서 더 이해를 해봐야 할 것 같습니다.

---

#### 나의 풀이

~~~python
n = int(input())
graph = []
INF = int(10e9)

for i in range(n):
    graph.append([g if g else INF for g in map(int, input().split())])

for k in range(n):
    for i in range(n):
        for j in range(n):
            graph[i][j] = min(graph[i][j], graph[i][k] + graph[k][j])

for i in range(n):
    for j in range(n):
        print(1 if graph[i][j] != INF else 0, end=' ')
    print()
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/31943505

~~~python
n=int(input())
li=[list(map(int,input().split())) for i in range(n)]
for k in range(n):
    for j in range(n):
        for i in range(n):
            if li[i][k]==1 and li[k][j]==1:
                li[i][j]=1
for i in li:
    print(*i)
~~~
