# ABCDE (Gold 5)

### 문제

BOJ 알고리즘 캠프에는 총 N명이 참가하고 있다. 사람들은 0번부터 N-1번으로 번호가 매겨져 있고, 일부 사람들은 친구이다.

오늘은 다음과 같은 친구 관계를 가진 사람 A, B, C, D, E가 존재하는지 구해보려고 한다.

* A는 B와 친구다.
* B는 C와 친구다.
* C는 D와 친구다.
* D는 E와 친구다.

위와 같은 친구 관계가 존재하는지 안하는지 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 사람의 수 N (5 ≤ N ≤ 2000)과 친구 관계의 수 M (1 ≤ M ≤ 2000)이 주어진다.

둘째 줄부터 M개의 줄에는 정수 a와 b가 주어지며, a와 b가 친구라는 뜻이다. (0 ≤ a, b ≤ N-1, a ≠ b) 같은 친구 관계가 두 번 이상 주어지는 경우는 없다.

---

#### 출력

문제의 조건에 맞는 A, B, C, D, E가 존재하면 1을 없으면 0을 출력한다.

출처 : https://www.acmicpc.net/problem/13023

---

### 문제풀이

이 문제는 백트래킹 문제입니다.

평소대로 문제를 풀었을 때 풀리지 않아서 조금 고생했던 문제입니다.

문제의 기본 풀이는 dfs로 탐색후 길이가 5이상인 경로가 존재하면 정답처리 후 종료하면 됩니다.

그런데 중간에 탐색에 실패한 경로가 존재하면 해당 경로를 초기화 해서 다음 탐색 때 포함하도록 해야 합니다.

이런 유형의 문제는 처음이라서 조금 생각을 하고 문제를 풀어야 했습니다.

---

#### 나의 풀이

~~~python
import java.io.*;
import java.util.*;
import java.util.stream.IntStream;

public class Main {
    static List<List<Integer>> graph = new ArrayList<>();
    static boolean[] visited;
    static boolean hasAnswer;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        IntStream.range(0, n).forEachOrdered(i -> graph.add(new ArrayList<>()));

        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            graph.get(a).add(b);
            graph.get(b).add(a);
        }

        for (int i = 0; i < n; i++) {
            visited = new boolean[n];
            dfs(1, i);
            if (hasAnswer) {
                System.out.println(1);
                return;
            }
        }

        System.out.println(0);
    }

    static void dfs(int depth, int current) {
        if (depth >= 5) {
            hasAnswer = true;
            return;
        }

        visited[current] = true;

        for (Integer next : graph.get(current)) {
            if (!visited[next]) {
                dfs(depth + 1, next);
            }
        }

        visited[current] = false;
    }
}
~~~
