# 스네이크버드 (Silver 5)

### 문제 설명

스네이크버드는 뱀과 새의 모습을 닮은 귀여운 생물체입니다. 

스네이크버드의 주요 먹이는 과일이며 과일 하나를 먹으면 길이가 1만큼 늘어납니다.

과일들은 지상으로부터 일정 높이를 두고 떨어져 있으며 i (1 ≤ i ≤ N) 번째 과일의 높이는 hi입니다. 

스네이크버드는 자신의 길이보다 작거나 같은 높이에 있는 과일들을 먹을 수 있습니다.

스네이크버드의 처음 길이가 L일때 과일들을 먹어 늘릴 수 있는 최대 길이를 구하세요.

출처 : https://www.acmicpc.net/problem/16435

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int l = scanner.nextInt();
        int[] fruits = new int[n];
        for (int i = 0; i < n; i++) {
            fruits[i] = scanner.nextInt();
        }

        System.out.println(getSnakeBirdSize(l, fruits));
    }

    static int getSnakeBirdSize(int snakeBirdSize, int[] fruits) {
        Arrays.sort(fruits);
        for (int fruit : fruits) {
            snakeBirdSize = increaseSnakeBirdSizeOrNot(snakeBirdSize, fruit);
        }
        return snakeBirdSize;
    }

    static int increaseSnakeBirdSizeOrNot(int snakeBirdSize, int fruit) {
        if (isEatableFruit(snakeBirdSize, fruit)) {
            snakeBirdSize++;
        }
        return snakeBirdSize;
    }

    static boolean isEatableFruit(int snakeBirdSize, int fruit) {
        return fruit <= snakeBirdSize;
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
    void getSnakeBirdSizeTest() {
        assertEquals(2, getSnakeBirdSize(1, new int[]{1}));
        assertEquals(1, getSnakeBirdSize(1, new int[]{3}));
        assertEquals(12, getSnakeBirdSize(10, new int[]{10, 11, 13}));
        assertEquals(12, getSnakeBirdSize(10, new int[]{13, 11, 10}));
        assertEquals(10, getSnakeBirdSize(1, new int[]{9, 5, 8, 1, 3, 2, 7, 6, 4}));
    }

    private int getSnakeBirdSize(int snakeBirdSize, int[] fruits) {
        Arrays.sort(fruits);
        for (int fruit : fruits) {
            snakeBirdSize = increaseSnakeBirdSizeOrNot(snakeBirdSize, fruit);
        }
        return snakeBirdSize;
    }

    private int increaseSnakeBirdSizeOrNot(int snakeBirdSize, int fruit) {
        if (isEatableFruit(snakeBirdSize, fruit)) {
            snakeBirdSize++;
        }
        return snakeBirdSize;
    }

    private boolean isEatableFruit(int snakeBirdSize, int fruit) {
        return fruit <= snakeBirdSize;
    }
}
~~~
