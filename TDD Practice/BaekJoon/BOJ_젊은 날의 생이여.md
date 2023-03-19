# 젊은 날의 생이여 (Gold 5)

### 문제 설명

욱제는 입대를 앞두고 <이등병의 편지>를 부르고 있다.

… 이제 다시 시작이다 젊은 날의 생이여 … 

그런데 과연 욱제에게 젊은 날이 있었을까? 욱제에게 젊은 날이 있었는지 알아보자.

먼저, N년간 욱제의 행복도와 피로도가 주어진다. 행복도와 피로도는 양의 실수 값을 가진다. 어떤 1 ≤ K < N에 대해 1, 2, …, K년은 욱제의 젊은 날이고, K + 1, K + 2, …, N년은 욱제의 늙은 날이다.

젊은 날과 늙은 날은 다음의 조건을 만족한다:

* 임의의 젊은 날의 행복도는 임의의 늙은 날의 행복도보다 높다.
* 임의의 젊은 날의 피로도는 임의의 늙은 날의 피로도보다 낮다.

욱제는 자신의 행복도와 피로도를 이용하여 자신의 젊은 날을 알아보려 한다. 하지만, 일부 값이 누락되었다. 욱제를 도와주자.

출처 : https://www.acmicpc.net/problem/18866

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int[][] happinessAndFatigue = new int[n][2];
        for (int i = 0; i < n; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            happinessAndFatigue[i][0] = Integer.parseInt(st.nextToken());
            happinessAndFatigue[i][1] = Integer.parseInt(st.nextToken());
        }

        System.out.println(getLongestYoungYear(happinessAndFatigue));
    }

    static int getLongestYoungYear(int[][] happinessAndFatigue) {
        int[][] youngestHappinessAndFatigue = getYoungestHappinessAndFatigue(happinessAndFatigue);
        int[][] oldestHappinessAndFatigue = getOldestHappinessAndFatigue(happinessAndFatigue);

        int longestYoungYear = -1;
        for (int i = 0; i < happinessAndFatigue.length - 1; i++) {
            longestYoungYear = increaseIfYoung(youngestHappinessAndFatigue, oldestHappinessAndFatigue, longestYoungYear, i);
        }
        return longestYoungYear;
    }

    static int[][] getYoungestHappinessAndFatigue(int[][] happinessAndFatigue) {
        int[][] youngestHappinessAndFatigue = new int[happinessAndFatigue.length][2];
        youngestHappinessAndFatigue[0][0] = getValueOrDefault(happinessAndFatigue[0][0], 1_999_999_999);
        youngestHappinessAndFatigue[0][1] = getValueOrDefault(happinessAndFatigue[0][1], -1);

        for (int i = 1; i < happinessAndFatigue.length; i++) {
            youngestHappinessAndFatigue[i][0] = Math.min(youngestHappinessAndFatigue[i-1][0], getValueOrDefault(happinessAndFatigue[i][0], youngestHappinessAndFatigue[i-1][0]));
            youngestHappinessAndFatigue[i][1] = Math.max(youngestHappinessAndFatigue[i-1][1], getValueOrDefault(happinessAndFatigue[i][1], youngestHappinessAndFatigue[i-1][1]));
        }
        return youngestHappinessAndFatigue;
    }

    static int[][] getOldestHappinessAndFatigue(int[][] happinessAndFatigue) {
        int[][] oldestHappinessAndFatigue = new int[happinessAndFatigue.length][2];
        oldestHappinessAndFatigue[happinessAndFatigue.length-1][0] = getValueOrDefault(happinessAndFatigue[happinessAndFatigue.length-1][0], 0);
        oldestHappinessAndFatigue[happinessAndFatigue.length-1][1] = getValueOrDefault(happinessAndFatigue[happinessAndFatigue.length-1][1], 1_999_999_999);

        for (int i = happinessAndFatigue.length - 2; i >= 0; i--) {
            oldestHappinessAndFatigue[i][0] = Math.max(oldestHappinessAndFatigue[i+1][0], getValueOrDefault(happinessAndFatigue[i][0], oldestHappinessAndFatigue[i+1][0]));
            oldestHappinessAndFatigue[i][1] = Math.min(oldestHappinessAndFatigue[i+1][1], getValueOrDefault(happinessAndFatigue[i][1], oldestHappinessAndFatigue[i+1][1]));
        }
        return oldestHappinessAndFatigue;
    }

    static int getValueOrDefault(int value, int defaultValue) {
        if (value == 0) {
            return defaultValue;
        }
        return value;
    }

    static int increaseIfYoung(int[][] youngestHappinessAndFatigue, int[][] oldestHappinessAndFatigue, int longestYoungYear, int year) {
        if (isYoung(youngestHappinessAndFatigue[year], oldestHappinessAndFatigue[year+1])) {
            longestYoungYear = year + 1;
        }
        return longestYoungYear;
    }

    static boolean isYoung(int[] young, int[] old) {
        int youngHappiness = young[0];
        int youngFatigue = young[1];
        int oldHappiness = old[0];
        int oldFatigue = old[1];
        return youngHappiness > oldHappiness && youngFatigue < oldFatigue;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getLongestYoungYearTest() {
        assertEquals(1, getLongestYoungYear(new int[][]{{2, 1}, {1, 2}}));
        assertEquals(-1, getLongestYoungYear(new int[][]{{1, 1}, {2, 2}}));
        assertEquals(3, getLongestYoungYear(new int[][]{{5, 1}, {4, 2}, {3, 3}, {2, 5}, {1, 4}}));
        assertEquals(4, getLongestYoungYear(new int[][]{{5, 1}, {4, 2}, {3, 3}, {2, 0}, {1, 4}}));
        assertEquals(-1, getLongestYoungYear(new int[][]{{5, 1}, {4, 2}, {3, 3}, {2, 0}, {1, 4}, {6, 0}}));
        assertEquals(2, getLongestYoungYear(new int[][]{{0, 0}, {0, 0}, {0, 0}}));
        assertEquals(-1, getLongestYoungYear(new int[][]{{1, 1}, {2, 2}, {3, 3}}));
    }

    private int getLongestYoungYear(int[][] happinessAndFatigue) {
        int[][] youngestHappinessAndFatigue = getYoungestHappinessAndFatigue(happinessAndFatigue);
        int[][] oldestHappinessAndFatigue = getOldestHappinessAndFatigue(happinessAndFatigue);

        int longestYoungYear = -1;
        for (int i = 0; i < happinessAndFatigue.length - 1; i++) {
            longestYoungYear = increaseIfYoung(youngestHappinessAndFatigue, oldestHappinessAndFatigue, longestYoungYear, i);
        }
        return longestYoungYear;
    }

    private int[][] getYoungestHappinessAndFatigue(int[][] happinessAndFatigue) {
        int[][] youngestHappinessAndFatigue = new int[happinessAndFatigue.length][2];
        youngestHappinessAndFatigue[0][0] = getValueOrDefault(happinessAndFatigue[0][0], 1_999_999_999);
        youngestHappinessAndFatigue[0][1] = getValueOrDefault(happinessAndFatigue[0][1], -1);

        for (int i = 1; i < happinessAndFatigue.length; i++) {
            youngestHappinessAndFatigue[i][0] = Math.min(youngestHappinessAndFatigue[i-1][0], getValueOrDefault(happinessAndFatigue[i][0], youngestHappinessAndFatigue[i-1][0]));
            youngestHappinessAndFatigue[i][1] = Math.max(youngestHappinessAndFatigue[i-1][1], getValueOrDefault(happinessAndFatigue[i][1], youngestHappinessAndFatigue[i-1][1]));
        }
        return youngestHappinessAndFatigue;
    }

    private int[][] getOldestHappinessAndFatigue(int[][] happinessAndFatigue) {
        int[][] oldestHappinessAndFatigue = new int[happinessAndFatigue.length][2];
        oldestHappinessAndFatigue[happinessAndFatigue.length-1][0] = getValueOrDefault(happinessAndFatigue[happinessAndFatigue.length-1][0], 0);
        oldestHappinessAndFatigue[happinessAndFatigue.length-1][1] = getValueOrDefault(happinessAndFatigue[happinessAndFatigue.length-1][1], 1_999_999_999);

        for (int i = happinessAndFatigue.length - 2; i >= 0; i--) {
            oldestHappinessAndFatigue[i][0] = Math.max(oldestHappinessAndFatigue[i+1][0], getValueOrDefault(happinessAndFatigue[i][0], oldestHappinessAndFatigue[i+1][0]));
            oldestHappinessAndFatigue[i][1] = Math.min(oldestHappinessAndFatigue[i+1][1], getValueOrDefault(happinessAndFatigue[i][1], oldestHappinessAndFatigue[i+1][1]));
        }
        return oldestHappinessAndFatigue;
    }

    private int getValueOrDefault(int value, int defaultValue) {
        if (value == 0) {
            return defaultValue;
        }
        return value;
    }

    private int increaseIfYoung(int[][] youngestHappinessAndFatigue, int[][] oldestHappinessAndFatigue, int longestYoungYear, int year) {
        if (isYoung(youngestHappinessAndFatigue[year], oldestHappinessAndFatigue[year+1])) {
            longestYoungYear = year + 1;
        }
        return longestYoungYear;
    }

    private boolean isYoung(int[] young, int[] old) {
        int youngHappiness = young[0];
        int youngFatigue = young[1];
        int oldHappiness = old[0];
        int oldFatigue = old[1];
        return youngHappiness > oldHappiness && youngFatigue < oldFatigue;
    }
}
~~~
