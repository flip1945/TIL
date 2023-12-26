# Triangle Height (Bronze 4)

### 문제 설명

Your Math teacher asked you to write a program to help him generate homework solutions. The program will need to find the height of a triangle given its area and its base length.

Formula:

* a = (h*b)/2 (a – area, b – base length, h – height)

출처 : https://www.acmicpc.net/problem/26592

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            TriangleHeight triangleHeight = new TriangleHeight(scanner.nextDouble(), scanner.nextDouble());
            System.out.println(triangleHeight);
        }
    }
}

class TriangleHeight {
    private static final String TRIANGLE_HEIGHT_FORMAT = "The height of the triangle is %.2f units";

    private final double area;
    private final double base;

    public TriangleHeight(double area, double base) {
        this.area = area;
        this.base = base;
    }

    public double getHeight() {
        return 2 * area / base;
    }

    @Override
    public String toString() {
        return String.format(TRIANGLE_HEIGHT_FORMAT, getHeight());
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideAreaAndBaseLength")
    @DisplayName("면적과 밑변의 길이가 주어지면 삼각형의 높이를 구한다.")
    void triangleHeightReturnsHeightOfTriangleGivenTriangleAreaAndBaseLength(double area, double base, String expected) {
        TriangleHeight sut = new TriangleHeight(area, base);

        String actual = sut.toString();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideAreaAndBaseLength() {
        return Stream.of(
                Arguments.of(200.533, 40.5, "The height of the triangle is 9.90 units"),
                Arguments.of(10.6, 1.11, "The height of the triangle is 19.10 units"),
                Arguments.of(30, 30, "The height of the triangle is 2.00 units"),
                Arguments.of(3333, 50.7, "The height of the triangle is 131.48 units")
        );
    }
}

class TriangleHeight {
    private static final String TRIANGLE_HEIGHT_FORMAT = "The height of the triangle is %.2f units";

    private final double area;
    private final double base;

    public TriangleHeight(double area, double base) {
        this.area = area;
        this.base = base;
    }

    public double getHeight() {
        return 2 * area / base;
    }

    @Override
    public String toString() {
        return String.format(TRIANGLE_HEIGHT_FORMAT, getHeight());
    }
}
~~~
