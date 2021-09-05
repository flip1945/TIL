# 달빛 여우 (Gold 1)

### 문제 설명

관악산 기슭에는 보름달을 기다리는 달빛 여우가 한 마리 살고 있다. 달빛 여우가 보름달의 달빛을 받으면 아름다운 구미호로 변신할 수 있다. 하지만 보름달을 기다리는 건 달빛 여우뿐만이 아니다. 달빛을 받아서 멋진 늑대인간이 되고 싶어 하는 달빛 늑대도 한 마리 살고 있다.   

관악산에는 1번부터 N번까지의 번호가 붙은 N개의 나무 그루터기가 있고, 그루터기들 사이에는 M개의 오솔길이 나 있다. 오솔길은 어떤 방향으로든 지나갈 수 있으며, 어떤 두 그루터기 사이에 두 개 이상의 오솔길이 나 있는 경우는 없다. 달빛 여우와 달빛 늑대는 1번 나무 그루터기에서 살고 있다.   

보름달이 뜨면 나무 그루터기들 중 하나가 달빛을 받아 밝게 빛나게 된다. 그러면 달빛 여우와 달빛 늑대는 먼저 달빛을 독차지하기 위해 최대한 빨리 오솔길을 따라서 그 그루터기로 달려가야 한다. 이때 달빛 여우는 늘 일정한 속도로 달려가는 반면, 달빛 늑대는 달빛 여우보다 더 빠르게 달릴 수 있지만 체력이 부족하기 때문에 다른 전략을 사용한다. 달빛 늑대는 출발할 때 오솔길 하나를 달빛 여우의 두 배의 속도로 달려가고, 그 뒤로는 오솔길 하나를 달빛 여우의 절반의 속도로 걸어가며 체력을 회복하고 나서 다음 오솔길을 다시 달빛 여우의 두 배의 속도로 달려가는 것을 반복한다. 달빛 여우와 달빛 늑대는 각자 가장 빠르게 달빛이 비치는 그루터기까지 다다를 수 있는 경로로 이동한다. 따라서 둘의 이동 경로가 서로 다를 수도 있다.   

출제자는 관악산의 모든 동물을 사랑하지만, 이번에는 달빛 여우를 조금 더 사랑해 주기로 했다. 그래서 달빛 여우가 달빛 늑대보다 먼저 도착할 수 있는 그루터기에 달빛을 비춰 주려고 한다. 이런 그루터기가 몇 개나 있는지 알아보자.

---

#### 입력

첫 줄에 나무 그루터기의 개수와 오솔길의 개수를 의미하는 정수 N, M(2 ≤ N ≤ 4,000, 1 ≤ M ≤ 100,000)이 주어진다.   

두 번째 줄부터 M개의 줄에 걸쳐 각 줄에 세 개의 정수 a, b, d(1 ≤ a, b ≤ N, a ≠ b, 1 ≤ d ≤ 100,000)가 주어진다. 이는 a번 그루터기와 b번 그루터기 사이에 길이가 d인 오솔길이 나 있음을 의미한다.

---

#### 출력

첫 줄에 달빛 여우가 달빛 늑대보다 먼저 도착할 수 있는 나무 그루터기의 개수를 출력한다.

---

#### 예제 입력 1

~~~
5 6
1 2 3
1 3 2
2 3 2
2 4 4
3 5 4
4 5 3
~~~

#### 예제 출력 1

~~~
1
~~~

---

#### 노트

<img src="https://upload.acmicpc.net/485e04a2-aaba-4e2b-aa95-ae655e2e00ba/-/preview/" width=395>

5번 그루터기에 달빛을 비추면 달빛 여우가 달빛 늑대보다 먼저 도착할 수 있다. 4번 그루터기에 달빛을 비추면 달빛 여우와 달빛 늑대가 동시에 도착한다.

[출처](https://www.acmicpc.net/problem/16118)

---

### 문제풀이

이번 문제는 그래프에서 여우와 늑대의 최단거리를 구하는 문제입니다.   

여우는 속도가 일정하기 때문에 일반적인 최단거리를 구하는 알고리즘을 사용하면 됩니다.   

하지만 늑대는 속도가 늘었다가 줄었다가 하기 때문에 일반적인 다익스트라 알고리즘으로는 풀 수 없습니다.   

저는 늑대의 최단거리를 구하기 위해 늑대의 최단거리를 구하는 리스트를 2개로 나눴습니다.   

그리고 빠른 속도로 도착했을 때와 느린 속도로 도착했을 때의 최단거리를 각각 저장해줬습니다.   

이 문제를 풀 때 주의할 점은 늑대의 시작점의 최단거리를 1개만 0으로 초기화 해야 하는 다는 점입니다.   

즉, 처음 출발은 빠른 속도로 출발하기 때문에 시작점의 느린 속도로 도착한 부분만 0으로 초기화 해야 한다는 말입니다.   

왜냐하면 다시 출발점으로 돌아오는 경우가 더 빠른 경우가 존재하기 때문입니다.   

일반적인 다익스트라 알고리즘은 다시 시작점으로 돌아올 일이 없지만   

늑대는 속도가 변하기 때문에 돌아서 가는 경우가 최단 거리가 더 짧은 경우가 생기기 때문입니다.   

ps. 저는 처음에 이 문제를 python으로 풀었는데, 맞는 알고리즘인데도 계속 시간 초과가 나와서 왜 그런지 검색을 해봤습니다.   

이 문제가 python을 염두하고 만들어진 문제가 아니라서 시간 초과 판정이 빠듯하게 설정되있었습니다.   

그래서 결국에는 같은 알고리즘을 java로 바꿔서 풀었고, 정답 판정을 받을 수 있었습니다.

python으로 푼 다른 분의 풀이를 보니 저랑 다른 점은 graph를 설정할 때, 2차원 graph가 아닌 1차원 graph로 설정하고   

graph를 2개로 나눠서 문제를 풀었다는 점인 것 같습니다.   

왜 이렇게 속도 차이가 극명하게 나는지 더 공부해야 겠습니다.

---

#### 나의 풀이

1. python으로 푼 풀이(통과 못함)

~~~python
import sys, heapq

n, m = map(int, sys.stdin.readline().split())
graph = [[] for _ in range(n + 1)]
INF = int(10e9)
# graph 생성
for _ in range(m):
    a, b, d = map(int, sys.stdin.readline().split())
    # 늑대의 속도가 절반 혹은 두 배로 변하으로 계산을 편하게 하기 위해 거리에 2를 곱한 값으로 graph 초기화함
    graph[a].append((b, d * 2))
    graph[b].append((a, d * 2))

dist_fox = [INF] * (n + 1)
dist_fox[1] = 0
heap = [(0, 1)]
# 여우의 최단거리를 구하는 일반적인 다익스트라 알고리즘 실행
while heap:
    cur_dist, cur_node = heapq.heappop(heap)

    if dist_fox[cur_node] < cur_dist:
        continue

    for node, d in graph[cur_node]:
        cost = dist_fox[cur_node] + d
        if cost < dist_fox[node]:
            dist_fox[node] = cost
            heapq.heappush(heap, (cost, node))

dist_wolf = [[INF, INF] for _ in range(n + 1)]
dist_wolf[1][0] = 0
heap = [(0, 1, 1)]
# 늑대의 최단거리를 구하는 다익스트라 알고리즘 실행
while heap:
    cur_dist, cur_node, cur_speed = heapq.heappop(heap)

    if cur_speed and dist_wolf[cur_node][0] < cur_dist:
        continue
    elif not cur_speed and dist_wolf[cur_node][1] < cur_dist:
        continue
    # 현재 속도를 0과 1로 구분해서 1인 경우 현재 거리를 절반으로, 0인경우 현재 거리를 두 배로 만들어줌
    for node, d in graph[cur_node]:
        cost = dist_wolf[cur_node][0] + (d // 2) if cur_speed else dist_wolf[cur_node][1] + (d * 2)
        if cost < dist_wolf[node][cur_speed]:
            dist_wolf[node][cur_speed] = cost
            # 현재 속도에 XOR 연산을 해서 0 -> 1로, 1 -> 0으로 바꿔서 힙에 넣어줌
            heapq.heappush(heap, (cost, node, cur_speed^1))

print(sum(dist_fox[i] < min(dist_wolf[i]) for i in range(1, n + 1)))
~~~

2. java로 푼 풀이(통과)

~~~java
import java.io.*;
import java.util.*;

class Main {
    static int n, m;
    static final int INF = (int)10e9;
    static List<Node>[] graph = new ArrayList[4001];
    static int[][] distWolf = new int[4001][2];
    static int[] distFox = new int[4001];
    
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()); 
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        for (int i = 1; i <= n; i++) {
            graph[i] = new ArrayList<>();
            distFox[i] = INF;
            Arrays.fill(distWolf[i], INF);
        }
        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int d = Integer.parseInt(st.nextToken());
            graph[a].add(new Node(b, d * 2));
            graph[b].add(new Node(a, d * 2));
        }
        dijkstraFox();
        dijkstraWolf();
        int answer = 0;
        for (int i = 0; i < n + 1; i++) {
            if (distFox[i] < Math.min(distWolf[i][0], distWolf[i][1])) answer++;
        }
        System.out.println(answer);
    }
    
    static void dijkstraFox() {
        Queue<Node> heap = new PriorityQueue<>();
        distFox[1] = 0;
        heap.add(new Node(1, 0));
        while (!heap.isEmpty()) {
            Node tempNode = heap.poll();
            int curNode = tempNode.nextNode;
            int curDist = tempNode.dist;
            if (distFox[curNode] < curDist) continue;
            for (Node node : graph[curNode]) {
                int d = node.dist;
                int next = node.nextNode;
                int cost = distFox[curNode] + d;
                if (cost < distFox[next]) {
                    distFox[next] = cost;
                    heap.add(new Node(next, cost));
                }
            }
        }
    }
    
    static void dijkstraWolf() {
        Queue<Node> heap = new PriorityQueue<>();
        distWolf[1][0] = 0;
        heap.add(new Node(1, 0, 1));
        while (!heap.isEmpty()) {
            Node tempNode = heap.poll();
            int curNode = tempNode.nextNode;
            int curDist = tempNode.dist;
            int curSpeed = tempNode.speed;
            if (curSpeed == 1 && distWolf[curNode][0] < curDist) continue;
            else if (curSpeed == 0 && distWolf[curNode][1] < curDist) continue;
            for (Node node : graph[curNode]) {
                int d = node.dist;
                int next = node.nextNode;
                int cost = (curSpeed == 1) ? distWolf[curNode][0] + (d / 2) : distWolf[curNode][1] + (d * 2);
                if (cost < distWolf[next][curSpeed]) {
                    distWolf[next][curSpeed] = cost;
                    heap.add(new Node(next, cost, curSpeed^1));
                }
            }
        }
    }
    
    static class Node implements Comparable<Node> {
        int nextNode, dist, speed;
        public Node(int nextNode, int dist) {
            this.nextNode = nextNode;
            this.dist = dist;
        }
        public Node(int nextNode, int dist, int speed) {
            this.nextNode = nextNode;
            this.dist = dist;
            this.speed = speed;
        }
        @Override
        public int compareTo(Node o) {
            return dist - o.dist;
        }
    }
}
~~~

---

#### 다른 사람의 풀이

~~~python
input = __import__('sys').stdin.readline
MIS = lambda: map(int,input().split())

from heapq import *
def dijkstra(adj, source):
    n = len(adj)-1; dist = [float('inf')]*(n+1)
    dist[source] = 0; PQ = [(0, source)]
    while PQ:
        d, u = heappop(PQ)
        if dist[u] != d: continue
        for v, c in adj[u]:
            if dist[v] > d+c: dist[v] = d+c; heappush(PQ, (d+c, v))
    return dist

n, m = MIS()
G1 = [[] for i in range(n+1)]
G2 = [[] for i in range(2*n+1)]
for i in range(m):
    a, b, c = MIS()
    G1[a].append((b, 2*c))
    G1[b].append((a, 2*c))
    G2[a].append((n+b, c))
    G2[n+a].append((b, 4*c))
    G2[b].append((n+a, c))
    G2[n+b].append((a, 4*c))
d1 = dijkstra(G1, 1)
d2 = dijkstra(G2, 1)
print(sum(d1[i] < min(d2[i], d2[i+n]) for i in range(1, n+1)))
~~~
