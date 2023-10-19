# 자전거 속도 (Bronze 3)

### 문제 설명

대부분의 자전거 속도계는 앞 포크에 설치된 홀 효과 센서로 동작한다. 자석이 앞 바퀴의 포크중 하나에 부착되어, 홀 효과를 이용해 속도계가 바퀴의 회전수를 측정한다. 따라서 바퀴의 지름을 안다면 회전수를 통해 이동 거리를 측정할 수 있다. 또한 바퀴가 회전하는 동안 걸린 시간을 안다면 평균 속도 역시 알 수 있다.

바퀴의 지름, 회전수, 걸린 시간이 주어졌을 때, 총 이동 거리와 평균 속도를 계산하여라. 앞바퀴는 땅에서 떨어지거나 미끄러지거나 공전하지 않았다고 가정한다.

이동 거리의 단위는 miles이고, 평균 속도의 단위는 miles/hour 이다.

출처 : https://www.acmicpc.net/problem/2765

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        double diameter = scanner.nextDouble();
        int revolution = scanner.nextInt();
        double seconds = scanner.nextDouble();
        int index = 1;

        while (revolution != 0) {
            Bicycle bicycle = new Bicycle(diameter, revolution, seconds);
            System.out.printf("Trip #%d: %s", index++, bicycle.calculateDistanceAndSpeed());
            System.out.println();

            diameter = scanner.nextDouble();
            revolution = scanner.nextInt();
            seconds = scanner.nextDouble();
        }
    }
}

class Bicycle {

    private static final double PI = 3.1415927;
    private static final int MILE_PER_INCHES = 63_360;
    private static final int HOUR_PER_SECONDS = 3600;

    private final double diameter;
    private final int revolution;
    private final double seconds;

    public Bicycle(double diameter, int revolution, double seconds) {
        this.diameter = diameter;
        this.revolution = revolution;
        this.seconds = seconds;
    }

    public String calculateDistanceAndSpeed() {
        return String.format("%.2f %.2f", getDistanceOfMiles(), getSpeed(getDistanceOfMiles(), getHours()));
    }

    private double getSpeed(double distanceOfMiles, double hour) {
        return distanceOfMiles / hour;
    }

    private double getHours() {
        return seconds / HOUR_PER_SECONDS;
    }

    private double getDistanceOfMiles() {
        return getDistanceOfInches() / MILE_PER_INCHES;
    }

    private double getDistanceOfInches() {
        return diameter * PI * revolution;
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
    @MethodSource("provideBicycleInfo")
    @DisplayName("이동 거리와 평균 속도를 계산한다.")
    void bicycleCalculateDistanceAndMilesPerHour(double diameter, int revolution, double seconds, String expected) {
        Bicycle sut = new Bicycle(diameter, revolution, seconds);

        String actual = sut.calculateDistanceAndSpeed();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideBicycleInfo() {
        return Stream.of(
                Arguments.of(26, 1000, 5, "1.29 928.20"),
                Arguments.of(27.25, 873234, 3000, "1179.86 1415.84")
        );
    }
}

class Bicycle {

    private static final double PI = 3.1415927;
    private static final int MILE_PER_INCHES = 63_360;
    private static final int HOUR_PER_SECONDS = 3600;

    private final double diameter;
    private final int revolution;
    private final double seconds;

    public Bicycle(double diameter, int revolution, double seconds) {
        this.diameter = diameter;
        this.revolution = revolution;
        this.seconds = seconds;
    }

    public String calculateDistanceAndSpeed() {
        return String.format("%.2f %.2f", getDistanceOfMiles(), getSpeed(getDistanceOfMiles(), getHours()));
    }

    private double getSpeed(double distanceOfMiles, double hour) {
        return distanceOfMiles / hour;
    }

    private double getHours() {
        return seconds / HOUR_PER_SECONDS;
    }

    private double getDistanceOfMiles() {
        return getDistanceOfInches() / MILE_PER_INCHES;
    }

    private double getDistanceOfInches() {
        return diameter * PI * revolution;
    }
}
~~~
