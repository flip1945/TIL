# Absolutely (Bronze 4)

### 문제 설명

We will call the distance between any two values on the number line the absolute line distance. Given two values, find the absolute line distance and output it, rounded and formatted to one place of precision.

출처 : https://www.acmicpc.net/problem/26500

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            Distance distance = new Distance(scanner.nextDouble(), scanner.nextDouble());
            System.out.println(distance.getDistance());
        }
    }
}

class Distance {

    private static final String DISTANCE_FORMAT = "%.1f";

    private final double first;
    private final double second;

    public Distance(double first, double second) {
        this.first = first;
        this.second = second;
    }

    public String getDistance() {
        return String.format(DISTANCE_FORMAT, absolutelyDistance());
    }

    private double absolutelyDistance() {
        return Math.abs(first - second);
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

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideTwoNumbers")
    @DisplayName("두 개의 숫자가 주어지면 두 숫자 사이의 거리를 반환한다.")
    void distanceReturnsDistanceBetweenTwoNumbers(double first, double second, String expected) {
        Distance sut = new Distance(first, second);

        String actual = sut.getDistance();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideTwoNumbers() {
        return Stream.of(
                Arguments.of(1, 2, "1.0"),
                Arguments.of(3, 5, "2.0"),
                Arguments.of(8.1, -9, "17.1"),
                Arguments.of(-6.4, -18.34, "11.9")
        );
    }
}

class Distance {

    private static final String DISTANCE_FORMAT = "%.1f";

    private final double first;
    private final double second;

    public Distance(double first, double second) {
        this.first = first;
        this.second = second;
    }

    public String getDistance() {
        return String.format(DISTANCE_FORMAT, absolutelyDistance());
    }

    private double absolutelyDistance() {
        return Math.abs(first - second);
    }
}
~~~
