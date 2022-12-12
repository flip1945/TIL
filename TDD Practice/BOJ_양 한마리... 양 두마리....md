# 양 한마리... 양 두마리... (Silver 2)

### 문제 설명

얼마전에 나는 불면증에 시달렸지... 천장이 뚫어져라 뜬 눈으로 밤을 지새우곤 했었지.  그러던 어느 날 내 친구 광민이에게 나의 불면증에 대해 말했더니 이렇게 말하더군. "양이라도 세봐!"  정말 도움이 안되는 친구라고 생각했었지. 그런데 막상 또 다시 잠을 청해보려고 침대에 눕고 보니 양을 세고 있더군... 그런데 양을 세다보니 이걸로 프로그램을 하나 짜볼 수 있겠단 생각이 들더군 후후후... 그렇게 나는 침대에서 일어나 컴퓨터 앞으로 향했지.

양을 # 으로 나타내고 . 으로 풀을 표현하는 거야. 서로 다른 # 두 개 이상이 붙어있다면 한 무리의 양들이 있는거지. 그래... 좋았어..! 이걸로 초원에서 풀을 뜯고 있는 양들을 그리드로 표현해 보는거야!

그렇게 나는 양들을 그리드로 표현하고 나니까 갑자기 졸렵기 시작했어. 하지만 난 너무 궁금했지. 내가 표현한 그 그리드 위에 몇 개의 양무리가 있었는지! 그래서 나는 동이 트기 전까지 이 프로그램을 작성하고 장렬히 전사했지. 다음날 내가 잠에서 깨어났을 때 내 모니터에는 몇 개의 양무리가 있었는지 출력되어 있었지.

출처 : https://www.acmicpc.net/problem/11123

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    private static final String SHEEP = "#";

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = Integer.parseInt(scanner.nextLine());

        for (int i = 0; i < t; i++) {
            StringTokenizer st = new StringTokenizer(scanner.nextLine());
            int h = Integer.parseInt(st.nextToken());
            int w = Integer.parseInt(st.nextToken());
            String[][] field = new String[h][w];
            for (int j = 0; j < h; j++) {
                field[j] = scanner.nextLine().split("");
            }

            System.out.println(getCountOfSheepGroup(field, h, w));
        }
    }

    private static int getCountOfSheepGroup(String[][] field, int h, int w) {
        int countOfSheepGroup = 0;
        boolean[][] visited = new boolean[h][w];
        for (int i = 0; i < h; i++) {
            for (int j = 0; j < w; j++) {
                if (!visited[i][j] && field[i][j].equals(SHEEP)) {
                    countOfSheepGroup++;
                    bfs(field, visited, h, w, i, j);
                }
            }
        }
        return countOfSheepGroup;
    }

    private static void bfs(String[][] field, boolean[][] visited, int h, int w, int x, int y) {
        int[] dx = {1, 0, -1, 0};
        int[] dy = {0, -1, 0, 1};
        Queue<Point> queue = new LinkedList<>();
        queue.offer(new Point(x, y));
        while (!queue.isEmpty()) {
            Point currentPoint = queue.poll();
            for (int i = 0; i < 4; i++) {
                int nextX = currentPoint.x + dx[i];
                int nextY = currentPoint.y + dy[i];
                if (isValidPoint(h, w, nextX, nextY) && !visited[nextX][nextY] && field[nextX][nextY].equals(SHEEP)) {
                    queue.offer(new Point(nextX, nextY));
                    visited[nextX][nextY] = true;
                }
            }
        }
    }

    private static boolean isValidPoint(int h, int w, int currentX, int currentY) {
        return currentX >= 0 && currentX < h && currentY >= 0 && currentY < w;
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

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.*;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {
    private static final String SHEEP = "#";

    @Test
    void bfsTest() {
        String[][] field1 = {{"#", ".", "#", "."}, {".", "#", ".", "#"}, {"#", ".", "#", "#"}, {".", "#", ".", "#"}};
        String[][] field2 = {{"#", "#", "#", ".", "#"}, {".", ".", "#", ".", "."}, {"#", ".", "#", "#", "#"}};
        String[][] field3 = {{"#"}};
        String[][] field4 = {{"."}};
        String[][] field5 = {{"#", "#"}, {"#", "#"}};

        assertEquals(6, getCountOfSheepGroup(field1, 4, 4));
        assertEquals(3, getCountOfSheepGroup(field2, 3, 5));
        assertEquals(1, getCountOfSheepGroup(field3, 1, 1));
        assertEquals(0, getCountOfSheepGroup(field4, 1, 1));
        assertEquals(1, getCountOfSheepGroup(field5, 2, 2));
    }

    private int getCountOfSheepGroup(String[][] field, int h, int w) {
        int countOfSheepGroup = 0;
        boolean[][] visited = new boolean[h][w];
        for (int i = 0; i < h; i++) {
            for (int j = 0; j < w; j++) {
                if (!visited[i][j] && field[i][j].equals(SHEEP)) {
                    countOfSheepGroup++;
                    bfs(field, visited, h, w, i, j);
                }
            }
        }
        return countOfSheepGroup;
    }

    private void bfs(String[][] field, boolean[][] visited, int h, int w, int x, int y) {
        int[] dx = {1, 0, -1, 0};
        int[] dy = {0, -1, 0, 1};
        Queue<Point> queue = new LinkedList<>();
        queue.offer(new Point(x, y));
        while (!queue.isEmpty()) {
            Point currentPoint = queue.poll();
            for (int i = 0; i < 4; i++) {
                int nextX = currentPoint.x + dx[i];
                int nextY = currentPoint.y + dy[i];
                if (isValidPoint(h, w, nextX, nextY) && !visited[nextX][nextY] && field[nextX][nextY].equals(SHEEP)) {
                    queue.offer(new Point(nextX, nextY));
                    visited[nextX][nextY] = true;
                }
            }
        }
    }

    private boolean isValidPoint(int h, int w, int currentX, int currentY) {
        return currentX >= 0 && currentX < h && currentY >= 0 && currentY < w;
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
