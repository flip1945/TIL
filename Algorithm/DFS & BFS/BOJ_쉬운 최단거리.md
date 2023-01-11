# 쉬운 최단거리 (Silver 1)

### 문제

지도가 주어지면 모든 지점에 대해서 목표지점까지의 거리를 구하여라.

문제를 쉽게 만들기 위해 오직 가로와 세로로만 움직일 수 있다고 하자.

---

#### 입력

지도의 크기 n과 m이 주어진다. n은 세로의 크기, m은 가로의 크기다.(2 ≤ n ≤ 1000, 2 ≤ m ≤ 1000)

다음 n개의 줄에 m개의 숫자가 주어진다. 0은 갈 수 없는 땅이고 1은 갈 수 있는 땅, 2는 목표지점이다. 입력에서 2는 단 한개이다.

---

#### 출력

각 지점에서 목표지점까지의 거리를 출력한다. 원래 갈 수 없는 땅인 위치는 0을 출력하고, 원래 갈 수 있는 땅인 부분 중에서 도달할 수 없는 위치는 -1을 출력한다.

출처 : https://www.acmicpc.net/problem/14940

---

### 문제풀이

이번 문제는 평범한 bfs 문제입니다.

모든 위치에서 목적지까지 걸리는 거리를 구하라는 것은 목적지에서 모든 위치까지의 거리를 구하는 것과 같습니다.

bfs를 이용하면 가까운 거리부터 탐색할 수 있는데, python으로 문제를 풀면 더 빨리 풀 수 있지만

연습을 위해서 java로 문제를 풀었습니다.

---

#### 나의 풀이

~~~python
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(bf.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        int[][] map = new int[n][m];

        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(bf.readLine());
            for (int j = 0; j < m; j++) {
                int number = Integer.parseInt(st.nextToken());
                if (number == 1) {
                    number = -1;
                }
                map[i][j] = number;
            }
        }

        boolean isFirst = true;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (isFirst && map[i][j] == 2) {
                    map[i][j] = 0;
                    bfs(map, n, m, new Point(i, j));
                    isFirst = false;
                }
            }
        }

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                System.out.print(map[i][j] + " ");
            }
            System.out.println();
        }
    }

    private static void bfs(int[][] map, int n, int m, Point startPoint) {
        List<Point> queue = new LinkedList<>();
        queue.add(startPoint);

        int[] dx = new int[]{1, 0, -1, 0};
        int[] dy = new int[]{0, 1, 0, -1};

        while (!queue.isEmpty()) {
            Point currentPoint = queue.remove(0);
            int x = currentPoint.x;
            int y = currentPoint.y;
            for (int i = 0; i < 4; i++) {
                int nextX = x + dx[i];
                int nextY = y + dy[i];
                if (isReachable(map, nextX, nextY, n, m)) {
                    map[nextX][nextY] = map[x][y] + 1;
                    queue.add(new Point(nextX, nextY));
                }
            }
        }
    }

    private static boolean isReachable(int[][] map, int nextX, int nextY, int n, int m) {
        return nextX >= 0 && nextX < n && nextY >= 0 && nextY < m && map[nextX][nextY] == -1;
    }
}

class Point {
    int x;
    int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
~~~
