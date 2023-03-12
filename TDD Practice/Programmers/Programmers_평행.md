# 평행 (Level 0)

### 문제 설명

점 네 개의 좌표를 담은 이차원 배열  dots가 다음과 같이 매개변수로 주어집니다.

* \[\[x1, y1], \[x2, y2], \[x3, y3], \[x4, y4]]

주어진 네 개의 점을 두 개씩 이었을 때, 두 직선이 평행이 되는 경우가 있으면 1을 없으면 0을 return 하도록 solution 함수를 완성해보세요.

#### 제한사항

* dots의 길이 = 4
* dots의 원소는 [x, y] 형태이며 x, y는 정수입니다.
* 0 ≤ x, y ≤ 100
* 서로 다른 두개 이상의 점이 겹치는 경우는 없습니다.
* 두 직선이 겹치는 경우(일치하는 경우)에도 1을 return 해주세요.
* 임의의 두 점을 이은 직선이 x축 또는 y축과 평행한 경우는 주어지지 않습니다.

출처 : https://programmers.co.kr/learn/courses/30/lessons/120875

---

#### 풀이

~~~java
import java.util.*;

class Solution {
    public int solution(int[][] dots) {
        int[][] combinations = {{0, 1, 2, 3}, {0, 2, 1, 3}, {0, 3, 1, 2}};
        for (int[] combination : combinations) {
            if (isSameGradient(dots, combination)) {
                return 1;
            }
        }
        return 0;
    }

    private boolean isSameGradient(int[][] dots, int[] dotPositions) {
        return getGradient(dots[dotPositions[0]], dots[dotPositions[1]]) == getGradient(dots[dotPositions[2]], dots[dotPositions[3]]);
    }

    private double getGradient(int[] dot1, int[] dot2) {
        return (double)(dot2[1] - dot1[1]) / (dot2[0] - dot1[0]);
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
    void getGradientTest() {
        int[] dot1 = new int[]{1, 4};
        int[] dot2 = new int[]{9, 2};
        int[] dot3 = new int[]{3, 8};
        int[] dot4 = new int[]{11, 6};

        assertEquals((double)-2/8, getGradient(dot1, dot2));
        assertEquals((double)4/2, getGradient(dot1, dot3));
        assertEquals((double)-6/6, getGradient(dot2, dot3));
        assertEquals((double)2/10, getGradient(dot1, dot4));
        assertEquals((double)2/10, getGradient(dot4, dot1));
    }

    @Test
    void solutionTest() {
        int[][] dots1 = new int[][]{{1, 4}, {9, 2}, {3, 8}, {11, 6}};
        int[][] dots2 = new int[][]{{3, 5}, {4, 1}, {2, 4}, {5, 10}};

        assertEquals(1, solution(dots1));
        assertEquals(0, solution(dots2));
    }

    private int solution(int[][] dots) {
        int[][] combinations = {{0, 1, 2, 3}, {0, 2, 1, 3}, {0, 3, 1, 2}};
        for (int[] combination : combinations) {
            if (isSameGradient(dots, combination)) {
                return 1;
            }
        }
        return 0;
    }

    private boolean isSameGradient(int[][] dots, int[] dotPositions) {
        return getGradient(dots[dotPositions[0]], dots[dotPositions[1]]) == getGradient(dots[dotPositions[2]], dots[dotPositions[3]]);
    }

    private double getGradient(int[] dot1, int[] dot2) {
        return (double)(dot2[1] - dot1[1]) / (dot2[0] - dot1[0]);
    }
}
~~~
