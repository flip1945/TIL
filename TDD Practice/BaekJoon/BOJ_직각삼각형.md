# 직각삼각형 (Silver 1)

### 문제 설명

2차원 평면에 N개의 점이 주어져 있다. 이 중에서 세 점을 골랐을 때, 직각삼각형이 몇 개나 있는지를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1711

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int[][] points = new int[n][2];
        for (int i = 0; i < n; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            points[i][0] = Integer.parseInt(st.nextToken());
            points[i][1] = Integer.parseInt(st.nextToken());
        }
        System.out.println(getCountOfRightTriangle(n, points));
    }

    static int getCountOfRightTriangle(int n, int[][] points) {
        int countOfRightTriangle = 0;
        for (int i = 0; i < n - 2; i++) {
            for (int j = i + 1; j < n - 1; j++) {
                for (int k = j + 1; k < n; k++) {
                    countOfRightTriangle = addCountIfRightTriangle(points, countOfRightTriangle, i, j, k);
                }
            }
        }
        return countOfRightTriangle;
    }

    static int addCountIfRightTriangle(int[][] points, int countOfRightTriangle, int i, int j, int k) {
        if (isRightTriangleTest(getSquaredSides(points[i], points[j], points[k]))) {
            countOfRightTriangle++;
        }
        return countOfRightTriangle;
    }

    static boolean isRightTriangleTest(long[] squaredSides) {
        long max = 0;
        for (long squaredSide : squaredSides) {
            max = Math.max(max, squaredSide);
        }
        return max * 2 == squaredSides[2] + squaredSides[0] + squaredSides[1];
    }

    static long[] getSquaredSides(int[] dot1, int[] dot2, int[] dot3) {
        return new long[]{getSide(dot1, dot2), getSide(dot1, dot3), getSide(dot2, dot3)};
    }

    static long getSide(int[] dot1, int[] dot2) {
        long xLength = Math.abs(dot1[0] - dot2[0]);
        long yLength = Math.abs(dot1[1] - dot2[1]);
        return (xLength * xLength) + (yLength * yLength);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Arrays;

import static org.junit.jupiter.api.Assertions.assertArrayEquals;
import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getLinesTest() {
        assertArrayEquals(new long[]{1L, 1L, 2L}, Arrays.stream(getSquaredSides(new int[]{0, 1}, new int[]{0, 0}, new int[]{1, 0})).sorted().toArray());
        assertArrayEquals(new long[]{1L, 1L, 2L}, Arrays.stream(getSquaredSides(new int[]{0, 0}, new int[]{1, 0}, new int[]{0, -1})).sorted().toArray());
        assertArrayEquals(new long[]{9L, 16L, 25L}, Arrays.stream(getSquaredSides(new int[]{0, 0}, new int[]{3, 0}, new int[]{0, 4})).sorted().toArray());
        assertArrayEquals(new long[]{25L, 144L, 169L}, Arrays.stream(getSquaredSides(new int[]{0, 0}, new int[]{5, 0}, new int[]{0, 12})).sorted().toArray());
    }

    @Test
    void isRightTriangleTest() {
        assertEquals(true, isRightTriangleTest(new long[]{1L, 1L, 2L}));
        assertEquals(false, isRightTriangleTest(new long[]{1L, 1L, 3L}));
        assertEquals(true, isRightTriangleTest(new long[]{9L, 16L, 25L}));
        assertEquals(true, isRightTriangleTest(new long[]{25L, 144L, 169L}));
        assertEquals(false, isRightTriangleTest(new long[]{9L, 9L, 16L}));
    }

    private boolean isRightTriangleTest(long[] squaredSides) {
        long max = 0;
        for (long squaredSide : squaredSides) {
            max = Math.max(max, squaredSide);
        }
        return max * 2 == squaredSides[2] + squaredSides[0] + squaredSides[1];
    }

    private long[] getSquaredSides(int[] dot1, int[] dot2, int[] dot3) {
        return new long[]{getSide(dot1, dot2), getSide(dot1, dot3), getSide(dot2, dot3)};
    }

    private long getSide(int[] dot1, int[] dot2) {
        long xLength = Math.abs(dot1[0] - dot2[0]);
        long yLength = Math.abs(dot1[1] - dot2[1]);
        return (xLength * xLength) + (yLength * yLength);
    }
}
~~~
