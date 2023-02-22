# 벽 부수고 이동하기 2 (Gold 3)

### 문제

N×M의 행렬로 표현되는 맵이 있다. 맵에서 0은 이동할 수 있는 곳을 나타내고, 1은 이동할 수 없는 벽이 있는 곳을 나타낸다. 당신은 (1, 1)에서 (N, M)의 위치까지 이동하려 하는데, 이때 최단 경로로 이동하려 한다. 최단경로는 맵에서 가장 적은 개수의 칸을 지나는 경로를 말하는데, 이때 시작하는 칸과 끝나는 칸도 포함해서 센다.

만약에 이동하는 도중에 벽을 부수고 이동하는 것이 좀 더 경로가 짧아진다면, 벽을 K개 까지 부수고 이동하여도 된다.

한 칸에서 이동할 수 있는 칸은 상하좌우로 인접한 칸이다.

맵이 주어졌을 때, 최단 경로를 구해 내는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 N(1 ≤ N ≤ 1,000), M(1 ≤ M ≤ 1,000), K(1 ≤ K ≤ 10)이 주어진다. 다음 N개의 줄에 M개의 숫자로 맵이 주어진다. (1, 1)과 (N, M)은 항상 0이라고 가정하자.

---

#### 출력

첫째 줄에 최단 거리를 출력한다. 불가능할 때는 -1을 출력한다.

출처 : https://www.acmicpc.net/problem/14442

---

### 문제풀이

이 문제는 bfs 문제입니다.

벽을 뚫고 간다는 점을 3차원 배열로 구성해서 해결하면 됩니다.

이 아이디어를 떠올리는 게 어렵지만 비슷한 유형의 문제를 풀었던 적이 있어서 쉽게 풀 수 있었습니다.

---

#### 나의 풀이

~~~python
import java.io.*;
import java.util.*;

public class Main {
    static int[][][] graph;
    static boolean[][][] visited;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        int k = Integer.parseInt(st.nextToken());

        graph = new int[n][m][k+1];
        visited = new boolean[n][m][k+1];

        for (int i = 0; i < n; i++) {
            String line = br.readLine();
            for (int j = 0; j < m; j++) {
                if (line.charAt(j) == '1') {
                    for (int l = 0; l < k + 1; l++) {
                        graph[i][j][l] = -1;
                    }
                }
            }
        }

        System.out.println(bfs(n, m, k));
    }

    static int bfs(int n, int m, int k) {
        int[] dx = new int[]{0, 1, 0, -1};
        int[] dy = new int[]{1, 0, -1, 0};

        visited[0][0][0] = true;

        List<Point> queue = new LinkedList<>();
        queue.add(new Point(0, 0, 0, 1));

        while (!queue.isEmpty()) {
            Point currentPoint = queue.remove(0);

            int currentX = currentPoint.x;
            int currentY = currentPoint.y;
            int currentWall = currentPoint.wall;
            int currentAnswer = currentPoint.answer;

            if (currentX == n - 1 && currentY == m - 1) {
                return currentAnswer;
            }

            for (int i = 0; i < 4; i++) {
                int nextX = currentX + dx[i];
                int nextY = currentY + dy[i];
                if (nextX >= 0 && nextX < n && nextY >= 0 && nextY < m) {
                    if (currentWall < k) {
                        if (graph[nextX][nextY][currentWall] == -1 && !visited[nextX][nextY][currentWall + 1]) {
                            visited[nextX][nextY][currentWall + 1] = true;
                            queue.add(new Point(nextX, nextY, currentWall + 1, currentAnswer + 1));
                        } else if (graph[nextX][nextY][currentWall] == 0 && !visited[nextX][nextY][currentWall]) {
                            visited[nextX][nextY][currentWall] = true;
                            queue.add(new Point(nextX, nextY, currentWall, currentAnswer + 1));
                        }
                    } else if (graph[nextX][nextY][currentWall] == 0 && !visited[nextX][nextY][currentWall]) {
                        visited[nextX][nextY][currentWall] = true;
                        queue.add(new Point(nextX, nextY, currentWall, currentAnswer + 1));
                    }
                }
            }
        }
        return -1;
    }
}

class Point {
    int x;
    int y;
    int wall;
    int answer;

    public Point(int x, int y, int wall, int answer) {
        this.x = x;
        this.y = y;
        this.wall = wall;
        this.answer = answer;
    }
}
~~~
