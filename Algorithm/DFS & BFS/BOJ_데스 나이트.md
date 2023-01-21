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
import java.util.*;

public class Main {
    static int[][] map;
    static int n;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        n = scanner.nextInt();
        int r1 = scanner.nextInt();
        int c1 = scanner.nextInt();
        int r2 = scanner.nextInt();
        int c2 = scanner.nextInt();

        map = new int[n][n];
        System.out.println(bfs(r1, c1, r2, c2));
    }

    static int bfs(int startRow, int startColumn, int goalRow, int goalColumn) {
        int[] dx = {-2, -2, 0, 0, 2, 2};
        int[] dy = {-1, 1, -2, 2, -1, 1};

        List<Point> queue = new LinkedList<>();
        queue.add(new Point(startRow, startColumn));
        while (!queue.isEmpty()) {
            Point currentPoint = queue.remove(0);
            int x = currentPoint.x;
            int y = currentPoint.y;

            if (x == goalRow && y == goalColumn) {
                return map[x][y];
            }

            for (int i = 0; i < 6; i++) {
                int nextX = x + dx[i];
                int nextY = y + dy[i];

                if (nextX >= 0 && nextX < n && nextY >= 0 && nextY < n && map[nextX][nextY] == 0) {
                    queue.add(new Point(nextX, nextY));
                    map[nextX][nextY] = map[x][y] + 1;
                }
            }
        }
        return -1;
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
