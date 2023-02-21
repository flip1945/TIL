# 사탕 게임 (Silver 2)

### 문제 설명

상근이는 어렸을 적에 "봄보니 (Bomboni)" 게임을 즐겨했다.

가장 처음에 N×N크기에 사탕을 채워 놓는다. 사탕의 색은 모두 같지 않을 수도 있다. 상근이는 사탕의 색이 다른 인접한 두 칸을 고른다. 그 다음 고른 칸에 들어있는 사탕을 서로 교환한다. 이제, 모두 같은 색으로 이루어져 있는 가장 긴 연속 부분(행 또는 열)을 고른 다음 그 사탕을 모두 먹는다.

사탕이 채워진 상태가 주어졌을 때, 상근이가 먹을 수 있는 사탕의 최대 개수를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/3085

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        String[][] candies = new String[n][n];
        for (int i = 0; i < n; i++) {
            candies[i] = scanner.nextLine().split("");
        }
        System.out.println(bomboni(n, candies));
    }

    static int bomboni(int n, String[][] candies) {
        int maxSize = getLongestCandySize(n, candies);
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n - 1; col++) {
                swap(candies, row, col, row, col + 1);
                maxSize = Math.max(maxSize, getLongestCandySize(n, candies));
                swap(candies, row, col, row, col + 1);
            }
        }

        for (int col = 0; col < n; col++) {
            for (int row = 0; row < n - 1; row++) {
                swap(candies, row, col, row + 1, col);
                maxSize = Math.max(maxSize, getLongestCandySize(n, candies));
                swap(candies, row, col, row + 1, col);
            }
        }
        return maxSize;
    }

    static void swap(String[][] candies, int row, int col, int nextRow, int nextCol) {
        String temp = candies[nextRow][nextCol];
        candies[nextRow][nextCol] = candies[row][col];
        candies[row][col] = temp;
    }

    static int getLongestCandySize(int n, String[][] candies) {
        return Math.max(getLongestCandySizeOfRows(n, candies), getLongestCandySizeOfColumns(n, candies));
    }

    static int getLongestCandySizeOfRows(int n, String[][] candies) {
        int longestCandySize = 0;
        for (int row = 0; row < n; row++) {
            longestCandySize = getLongestCandySizeOfLine(n, candies[row], longestCandySize);
        }
        return longestCandySize;
    }

    static int getLongestCandySizeOfLine(int n, String[] candies, int longestCandySize) {
        int currentCandyStreak = 0;
        int longestCandyStreakOfLine = 0;
        String prevCandy = candies[0];
        for (String candy : candies) {
            if (!candy.equals(prevCandy)) {
                currentCandyStreak = 0;
            }
            currentCandyStreak++;
            prevCandy = candy;
            longestCandyStreakOfLine = Math.max(longestCandyStreakOfLine, currentCandyStreak);
        }
        longestCandySize = Math.max(longestCandySize, longestCandyStreakOfLine);
        return longestCandySize;
    }

    static int getLongestCandySizeOfColumns(int n, String[][] candies) {
        int longestCandySize = 0;
        for (int col = 0; col < n; col++) {
            longestCandySize = getLongestCandySizeOfLine(n, getCandiesOfColumns(candies, col), longestCandySize);
        }
        return longestCandySize;
    }

    static String[] getCandiesOfColumns(String[][] candies, int column) {
        return Arrays.stream(candies).map(candyLine -> candyLine[column]).toArray(String[]::new);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Arrays;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getLongestCandySizeTest() {
        assertEquals(2, getLongestCandySize(3, new String[][]{{"C", "C", "P"}, {"C", "C", "P"}, {"P", "P", "C"}}));
        assertEquals(3, getLongestCandySize(3, new String[][]{{"C", "C", "C"}, {"C", "P", "P"}, {"P", "C", "P"}}));
        assertEquals(3, getLongestCandySize(3, new String[][]{{"C", "P", "C"}, {"C", "C", "P"}, {"C", "P", "P"}}));
        assertEquals(4, getLongestCandySize(4, new String[][]{{"P", "P", "P", "P"}, {"C", "Y", "Z", "Y"}, {"C", "C", "P", "Y"}, {"P", "P", "C", "C"}}));
        assertEquals(3, getLongestCandySize(5, new String[][]{{"Y", "C", "P", "Z", "Y"}, {"C", "Y", "Z", "Z", "P"}, {"C", "C", "P", "P", "P"}, {"Y", "C", "Y", "Z", "C"}, {"C", "P", "P", "Z", "Z"}}));
    }

    @Test
    void bomboniTest() {
        assertEquals(3, bomboni(3, new String[][]{{"C", "C", "P"}, {"C", "C", "P"}, {"P", "P", "C"}}));
        assertEquals(4, bomboni(4, new String[][]{{"P", "P", "P", "P"}, {"C", "Y", "Z", "Y"}, {"C", "C", "P", "Y"}, {"P", "P", "C", "C"}}));
        assertEquals(4, bomboni(5, new String[][]{{"Y", "C", "P", "Z", "Y"}, {"C", "Y", "Z", "Z", "P"}, {"C", "C", "P", "P", "P"}, {"Y", "C", "Y", "Z", "C"}, {"C", "P", "P", "Z", "Z"}}));
    }

    private int bomboni(int n, String[][] candies) {
        int maxSize = getLongestCandySize(n, candies);
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n - 1; col++) {
                swap(candies, row, col, row, col + 1);
                maxSize = Math.max(maxSize, getLongestCandySize(n, candies));
                swap(candies, row, col, row, col + 1);
            }
        }

        for (int col = 0; col < n; col++) {
            for (int row = 0; row < n - 1; row++) {
                swap(candies, row, col, row + 1, col);
                maxSize = Math.max(maxSize, getLongestCandySize(n, candies));
                swap(candies, row, col, row + 1, col);
            }
        }
        return maxSize;
    }

    private void swap(String[][] candies, int row, int col, int nextRow, int nextCol) {
        String temp = candies[nextRow][nextCol];
        candies[nextRow][nextCol] = candies[row][col];
        candies[row][col] = temp;
    }

    private int getLongestCandySize(int n, String[][] candies) {
        return Math.max(getLongestCandySizeOfRows(n, candies), getLongestCandySizeOfColumns(n, candies));
    }

    private int getLongestCandySizeOfRows(int n, String[][] candies) {
        int longestCandySize = 0;
        for (int row = 0; row < n; row++) {
            longestCandySize = getLongestCandySizeOfLine(n, candies[row], longestCandySize);
        }
        return longestCandySize;
    }

    private int getLongestCandySizeOfLine(int n, String[] candies, int longestCandySize) {
        int currentCandyStreak = 0;
        int longestCandyStreakOfLine = 0;
        String prevCandy = candies[0];
        for (String candy : candies) {
            if (!candy.equals(prevCandy)) {
                currentCandyStreak = 0;
            }
            currentCandyStreak++;
            prevCandy = candy;
            longestCandyStreakOfLine = Math.max(longestCandyStreakOfLine, currentCandyStreak);
        }
        longestCandySize = Math.max(longestCandySize, longestCandyStreakOfLine);
        return longestCandySize;
    }

    private int getLongestCandySizeOfColumns(int n, String[][] candies) {
        int longestCandySize = 0;
        for (int col = 0; col < n; col++) {
            longestCandySize = getLongestCandySizeOfLine(n, getCandiesOfColumns(candies, col), longestCandySize);
        }
        return longestCandySize;
    }

    private String[] getCandiesOfColumns(String[][] candies, int column) {
        return Arrays.stream(candies).map(candyLine -> candyLine[column]).toArray(String[]::new);
    }
}
~~~
