# 여러분의 다리가 되어 드리겠습니다! (Gold 5)

### 문제

선린월드에는 N개의 섬이 있다. 섬에는 1, 2, ..., N의 번호가 하나씩 붙어 있다. 그 섬들을 N - 1개의 다리가 잇고 있으며, 어떤 두 섬 사이든 다리로 왕복할 수 있다.

어제까지는 그랬다.

"왜 다리가 N - 1개밖에 없냐, 통행하기 불편하다"며 선린월드에 불만을 갖던 욱제가 다리 하나를 무너뜨렸다! 안 그래도 불편한 통행이 더 불편해졌다. 서로 왕복할 수 없는 섬들이 생겼기 때문이다. 일단 급한 대로 정부는 선린월드의 건축가를 고용해, 서로 다른 두 섬을 다리로 이어서 다시 어떤 두 섬 사이든 왕복할 수 있게 하라는 지시를 내렸다.

그런데 그 건축가가 당신이다! 안 그래도 천하제일 코딩대회에 참가하느라 바쁜데...

---

#### 입력

첫 줄에 정수 N이 주어진다. (2 ≤ N ≤ 300,000)

그 다음 N - 2개의 줄에는 욱제가 무너뜨리지 않은 다리들이 잇는 두 섬의 번호가 주어진다.

---

#### 출력

다리로 이을 두 섬의 번호를 출력한다. 여러 가지 방법이 있을 경우 그 중 아무거나 한 방법만 출력한다.

출처 : https://www.acmicpc.net/problem/17352

---

### 문제풀이

이번 문제는 dfs로 해결했습니다.

연결된 그룹이 2개로 나눠진다고 생각했고, 1번과 연결된 그룹과 그 외 그룹으로 나눠서 정답을 출력하도록 했습니다.

그래프가 연결된 것을 확인하기 위해서 dfs를 사용했고, 연습을 위해서 python이 아닌 java로 문제를 풀었습니다.

그런데 이 문제는 그래프 탐색으로 풀 수도 있지만 전형적인 유니온 파인드 문제였습니다.

그룹을 나누는 문제라서 유니온 파인드로 쉽게 풀 수 있는 문제입니다.

다음에 비슷한 문제를 만나면 유니온 파인드로 문제를 풀어봐야겠습니다.

---

#### 나의 풀이

~~~python
import java.io.*;
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
}
~~~

#### 다른 사람의 풀이

유니온 파인드 풀이

https://www.acmicpc.net/source/54303273
