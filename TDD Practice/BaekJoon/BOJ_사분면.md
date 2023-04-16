# 사분면 (Bronze 3)

### 문제 설명

2차원 좌표 상의 여러 점의 좌표 (x,y)가 주어졌을 때, 각 사분면과 축에 점이 몇 개 있는지 구하는 프로그램을 작성하시오.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/quad.png">

출처 : https://www.acmicpc.net/problem/9610

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int[] quadrants = new int[5];
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            quadrants[getQuadrant(scanner.nextInt(), scanner.nextInt())]++;
        }

        printQuadrantsStatistics(quadrants);
    }

    private static void printQuadrantsStatistics(int[] quadrants) {
        System.out.println("Q1: " + quadrants[1]);
        System.out.println("Q2: " + quadrants[2]);
        System.out.println("Q3: " + quadrants[3]);
        System.out.println("Q4: " + quadrants[4]);
        System.out.println("AXIS: " + quadrants[0]);
    }

    static int getQuadrant(int x, int y) {
        return Quadrant.valueOfPoint(new Point(x, y)).getQuadrant();
    }
}

enum Quadrant {
    FIRST(1, point -> point.x > 0 && point.y > 0),
    SECOND(2, point -> point.x < 0 && point.y > 0),
    THIRD(3, point -> point.x < 0 && point.y < 0),
    FOURTH(4, point -> point.x > 0 && point.y < 0),
    AXIS(0, point -> false);

    private final int quadrant;
    private final Predicate<Point> match;

    Quadrant(int quadrant, Predicate<Point> match) {
        this.quadrant = quadrant;
        this.match = match;
    }

    public int getQuadrant() {
        return quadrant;
    }

    public static Quadrant valueOfPoint(Point point) {
        return Arrays.stream(values())
                .filter(quadrant -> quadrant.match.test(point))
                .findFirst()
                .orElse(Quadrant.AXIS);
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

import java.util.Arrays;
import java.util.function.Predicate;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getQuadrantTest() {
        assertEquals(1, getQuadrant(1, 1));
        assertEquals(2, getQuadrant(-1, 1));
        assertEquals(3, getQuadrant(-1, -1));
        assertEquals(4, getQuadrant(1, -1));
        assertEquals(0, getQuadrant(0, -1));
        assertEquals(0, getQuadrant(0, 0));
    }

    private int getQuadrant(int x, int y) {
        return Quadrant.valueOfPoint(new Point(x, y)).getQuadrant();
    }
}

enum Quadrant {
    FIRST(1, point -> point.x > 0 && point.y > 0),
    SECOND(2, point -> point.x < 0 && point.y > 0),
    THIRD(3, point -> point.x < 0 && point.y < 0),
    FOURTH(4, point -> point.x > 0 && point.y < 0),
    AXIS(0, point -> false);

    private final int quadrant;
    private final Predicate<Point> match;

    Quadrant(int quadrant, Predicate<Point> match) {
        this.quadrant = quadrant;
        this.match = match;
    }

    public int getQuadrant() {
        return quadrant;
    }

    public static Quadrant valueOfPoint(Point point) {
        return Arrays.stream(values())
                .filter(quadrant -> quadrant.match.test(point))
                .findFirst()
                .orElse(Quadrant.AXIS);
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
