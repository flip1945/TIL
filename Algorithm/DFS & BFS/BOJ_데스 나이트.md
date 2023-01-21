# 데스 나이트 (Silver 1)

### 문제

게임을 좋아하는 큐브러버는 체스에서 사용할 새로운 말 "데스 나이트"를 만들었다. 데스 나이트가 있는 곳이 (r, c)라면, (r-2, c-1), (r-2, c+1), (r, c-2), (r, c+2), (r+2, c-1), (r+2, c+1)로 이동할 수 있다.

크기가 N×N인 체스판과 두 칸 (r1, c1), (r2, c2)가 주어진다. 데스 나이트가 (r1, c1)에서 (r2, c2)로 이동하는 최소 이동 횟수를 구해보자. 체스판의 행과 열은 0번부터 시작한다.

데스 나이트는 체스판 밖으로 벗어날 수 없다.

---

#### 입력

첫째 줄에 체스판의 크기 N(5 ≤ N ≤ 200)이 주어진다. 둘째 줄에 r1, c1, r2, c2가 주어진다.

---

#### 출력

첫째 줄에 데스 나이트가 (r1, c1)에서 (r2, c2)로 이동하는 최소 이동 횟수를 출력한다. 이동할 수 없는 경우에는 -1을 출력한다.

출처 : https://www.acmicpc.net/problem/16948

---

### 문제풀이

이번 문제는 bfs 문제입니다.

처음 문제를 봤을 때는 어렵다고 느껴졌지만 지문을 자세히 보니 6가지 방향으로 움직일 수 있는 말이 특정 지점에 도착할 수 있는지 탐색하는 문제였습니다.

도달할 수 있을 경우 최소 이동 를 물어봤기 때문에 bfs를 이용해 문제를 해결할 수 있었습니다.

---

#### 나의 풀이

~~~java
**import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(bf.readLine());
        List<List<Integer>> graph = new ArrayList<>();

        for (int i = 0; i <= n; i++) {
            graph.add(new ArrayList<>());
        }

        for (int i = 0; i < n - 2; i++) {
            StringTokenizer st = new StringTokenizer(bf.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            graph.get(a).add(b);
            graph.get(b).add(a);
        }

        boolean[] visited = new boolean[n + 1];

        visited[1] = true;
        dfs(graph, visited, 1);

        for (int i = 2; i <= n; i++) {
            if (!visited[i]) {
                System.out.print("1 " + i);
                break;
            }
        }
    }

    static void dfs(List<List<Integer>> graph, boolean[] visited, int currentNode) {
        for (Integer nextNode : graph.get(currentNode)) {
            if (!visited[nextNode]) {
                visited[nextNode] = true;
                dfs(graph, visited, nextNode);
            }
        }
    }
}**
~~~
