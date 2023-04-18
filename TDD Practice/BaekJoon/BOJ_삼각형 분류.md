# 삼각형 분류 (Bronze 3)

### 문제 설명

꿍은 오늘 학교에서 삼각형에 대해 배웠다. 삼각형은 변의 길이에 따라 다음과 같이 분류될 수 있다.

* 정삼각형(equilateral triangle)은 모든 변의 길이가 같다. 각 역시 60도로 모두 같다.
* 이등변삼각형(isosceles triangle)은 두 개의 변의 길이가 같다. 각 역시 두 개의 각의 크기가 같다.
* 부등변삼각형(scalene triangle)은 모든 변의 길이가 같지 않다. 각 역시 모두 다르다. 몇몇 부등변삼각형의 경우 직각삼각형이다.

수학선생님이 삼각형의 세 변의 길이를 가지고 삼각형을 분류하라는 숙제를 내줬는데 꿍은 정말 놀고싶다. 꿍이 놀수있도록 여러분이 도와주어라.

출처 : https://www.acmicpc.net/problem/9366

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = scanner.nextInt();
        for (int i = 1; i <= t; i++) {
            System.out.println("Case #" + i + ": " + Triangle.classify(scanner.nextInt(), scanner.nextInt(), scanner.nextInt()));
        }
    }
}

enum Triangle {
    // Enum의 순서를 주의
    EQUILATERAL("equilateral", sides -> sides.getShortestSide() == sides.getLongestSide()),
    INVALID("invalid!", sides -> sides.getLongestSide() >= sides.getMiddleSide() + sides.getShortestSide()),
    ISOSCELES("isosceles", sides -> sides.getShortestSide() == sides.getMiddleSide() || sides.getMiddleSide() == sides.getLongestSide()),
    SCALENE("scalene", sides -> true);

    private final String name;
    private final Predicate<SortedSides> match;

    Triangle(String name, Predicate<SortedSides> match) {
        this.name = name;
        this.match = match;
    }

    public static String classify(int side1, int side2, int side3) {
        return Triangle.valueOfSortedSides(new SortedSides(side1, side2, side3)).getName();
    }

    private static Triangle valueOfSortedSides(SortedSides sortedSides) {
        for (Triangle triangle : Triangle.values()) {
            if (triangle.match.test(sortedSides)) {
                return triangle;
            }
        }
        return SCALENE;
    }

    public String getName() {
        return this.name;
    }
}

class SortedSides {
    private final int[] sortedSides;

    public SortedSides(int side1, int side2, int side3) {
        sortedSides = getSortedSides(side1, side2, side3);
    }

    private int[] getSortedSides(int side1, int side2, int side3) {
        int[] sortedSides = new int[]{side1, side2, side3};
        Arrays.sort(sortedSides);
        return sortedSides;
    }

    public int getLongestSide() {
        return sortedSides[2];
    }

    public int getMiddleSide() {
        return sortedSides[1];
    }

    public int getShortestSide() {
        return sortedSides[0];
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
    void classifyTriangleTest() {
        assertEquals("equilateral", Triangle.classify(1, 1, 1));
        assertEquals("equilateral", Triangle.classify(3, 3, 3));
        assertEquals("invalid!", Triangle.classify(1, 1, 2));
        assertEquals("isosceles", Triangle.classify(7, 10, 7));
        assertEquals("scalene", Triangle.classify(7, 10, 8));
        assertEquals("isosceles", Triangle.classify(3, 3, 4));
        assertEquals("invalid!", Triangle.classify(6, 4, 2));
    }
}

enum Triangle {
    // Enum의 순서를 주의
    EQUILATERAL("equilateral", sides -> sides.getShortestSide() == sides.getLongestSide()),
    INVALID("invalid!", sides -> sides.getLongestSide() >= sides.getMiddleSide() + sides.getShortestSide()),
    ISOSCELES("isosceles", sides -> sides.getShortestSide() == sides.getMiddleSide() || sides.getMiddleSide() == sides.getLongestSide()),
    SCALENE("scalene", sides -> true);

    private final String name;
    private final Predicate<SortedSides> match;

    Triangle(String name, Predicate<SortedSides> match) {
        this.name = name;
        this.match = match;
    }

    public static String classify(int side1, int side2, int side3) {
        return Triangle.valueOfSortedSides(new SortedSides(side1, side2, side3)).getName();
    }

    private static Triangle valueOfSortedSides(SortedSides sortedSides) {
        for (Triangle triangle : Triangle.values()) {
            if (triangle.match.test(sortedSides)) {
                return triangle;
            }
        }
        return SCALENE;
    }

    public String getName() {
        return this.name;
    }
}

class SortedSides {
    private final int[] sortedSides;

    public SortedSides(int side1, int side2, int side3) {
        sortedSides = getSortedSides(side1, side2, side3);
    }

    private int[] getSortedSides(int side1, int side2, int side3) {
        int[] sortedSides = new int[]{side1, side2, side3};
        Arrays.sort(sortedSides);
        return sortedSides;
    }

    public int getLongestSide() {
        return sortedSides[2];
    }

    public int getMiddleSide() {
        return sortedSides[1];
    }

    public int getShortestSide() {
        return sortedSides[0];
    }
}
~~~
