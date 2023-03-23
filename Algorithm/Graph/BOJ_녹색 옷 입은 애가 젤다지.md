# 녹색 옷 입은 애가 젤다지? (Gold 4)

### 문제

젤다의 전설 게임에서 화폐의 단위는 루피(rupee)다. 그런데 간혹 '도둑루피'라 불리는 검정색 루피도 존재하는데, 이걸 획득하면 오히려 소지한 루피가 감소하게 된다!

젤다의 전설 시리즈의 주인공, 링크는 지금 도둑루피만 가득한 N x N 크기의 동굴의 제일 왼쪽 위에 있다. [0][0]번 칸이기도 하다. 왜 이런 곳에 들어왔냐고 묻는다면 밖에서 사람들이 자꾸 "젤다의 전설에 나오는 녹색 애가 젤다지?"라고 물어봤기 때문이다. 링크가 녹색 옷을 입은 주인공이고 젤다는 그냥 잡혀있는 공주인데, 게임 타이틀에 젤다가 나와있다고 자꾸 사람들이 이렇게 착각하니까 정신병에 걸릴 위기에 놓인 것이다.

하여튼 젤다...아니 링크는 이 동굴의 반대편 출구, 제일 오른쪽 아래 칸인 [N-1][N-1]까지 이동해야 한다. 동굴의 각 칸마다 도둑루피가 있는데, 이 칸을 지나면 해당 도둑루피의 크기만큼 소지금을 잃게 된다. 링크는 잃는 금액을 최소로 하여 동굴 건너편까지 이동해야 하며, 한 번에 상하좌우 인접한 곳으로 1칸씩 이동할 수 있다.

링크가 잃을 수밖에 없는 최소 금액은 얼마일까?

---

#### 입력

입력은 여러 개의 테스트 케이스로 이루어져 있다.

각 테스트 케이스의 첫째 줄에는 동굴의 크기를 나타내는 정수 N이 주어진다. (2 ≤ N ≤ 125) N = 0인 입력이 주어지면 전체 입력이 종료된다.

이어서 N개의 줄에 걸쳐 동굴의 각 칸에 있는 도둑루피의 크기가 공백으로 구분되어 차례대로 주어진다. 도둑루피의 크기가 k면 이 칸을 지나면 k루피를 잃는다는 뜻이다. 여기서 주어지는 모든 정수는 0 이상 9 이하인 한 자리 수다.

---

#### 출력

각 테스트 케이스마다 한 줄에 걸쳐 정답을 형식에 맞춰서 출력한다. 형식은 예제 출력을 참고하시오.

출처 : https://www.acmicpc.net/problem/4485

---

### 문제풀이

이번 문제는 최단 거리를 구하는 문제입니다.

지문은 복잡하지만 간단히 정리하면 출발지(0, 0)부터 목적지(n-1, n-1)까지 경로의 비용이 가장 적은 최단 거리를 구하는 문제입니다.

문제가 비용이 가장 적은 최단 거리를 구하는 것만 인지하면 그 다음부터는 다익스트라 알고리즘을 이용해 쉽게 해결할 수 있습니다.

Java로 다익스트라 알고리즘을 구현하는 게 익숙하지 않아 조금 어려웠지만 그걸 제외하면 어려운 문제는 아니였습니다.

---

#### 나의 풀이

~~~java
import java.io.*;
import java.util.*;

public class Main {
    static int[][] graph;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int index = 1;
        int n;
        while ((n = Integer.parseInt(br.readLine())) != 0) {
            initGraph(br, n);

            System.out.println("Problem " + index++ + ": " + dijkstra(n));
        }
    }

    private static int dijkstra(int n) {
        boolean[][] visited = new boolean[n][n];
        int[] dx = new int[]{0, 1, 0, -1};
        int[] dy = new int[]{1, 0, -1, 0};

        PriorityQueue<Point> queue = new PriorityQueue<>(Comparator.comparingInt(p -> p.cost));
        queue.add(new Point(0, 0, graph[0][0]));
        visited[0][0] = true;
        while (!queue.isEmpty()) {
            Point currentPoint = queue.poll();

            for (int i = 0; i < 4; i++) {
                int nextX = currentPoint.x + dx[i];
                int nextY = currentPoint.y + dy[i];

                if (nextX >= 0 && nextX < n && nextY >= 0 && nextY <n && !visited[nextX][nextY]) {
                    int nextCost = currentPoint.cost + graph[nextX][nextY];
                    queue.add(new Point(nextX, nextY, nextCost));
                    graph[nextX][nextY] = nextCost;
                    visited[nextX][nextY] = true;
                }
            }
        }
        return graph[n-1][n-1];
    }

    private static void initGraph(BufferedReader br, int n) throws IOException {
        graph = new int[n][n];
        for (int i = 0; i < n; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int j = 0; j < n; j++) {
                graph[i][j] = Integer.parseInt(st.nextToken());
            }
        }
    }
}

class Point {
    int x;
    int y;
    int cost;

    public Point(int x, int y, int cost) {
        this.x = x;
        this.y = y;
        this.cost = cost;
    }
}
~~~
