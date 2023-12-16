# Amusement Park Adventure (Bronze 4)

### 문제 설명

Meet Carlitos, a spirited adventure enthusiast with an insatiable love for amusement parks. Despite his vibrant passion, Carlitos faces a unique challenge – his height. As he eagerly plans his weekend escapade, he realizes that his vertical limitations might hinder his amusement park experience. It’s not just about choosing a park; it’s about finding one where he can enjoy the thrill of the rides.

Picture the kaleidoscope of colors, the jubilant laughter, and the heart-pounding rush of the rides. Carlitos has always been drawn to the energy of amusement parks. With the weekend approaching, he pores over park brochures, studying the height requirements of each ride. His goal is to maximize his enjoyment, and that’s where you come in.

Your task is to help Carlitos determine the number of rides he can enjoy at a specific park. By considering his height and the minimum height requirements of each ride, guide him in making the most of his amusement park adventure.

출처 : https://www.acmicpc.net/problem/29986

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int height = scanner.nextInt();
        AmusementPark amusementPark = new AmusementPark(inputHeightLimits(scanner, n));
        System.out.println(amusementPark.rideableCount(height));
    }

    private static List<Integer> inputHeightLimits(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class AmusementPark {

    private final List<Integer> heightLimitsOfRides;

    public AmusementPark(List<Integer> heightLimitsOfRides) {
        this.heightLimitsOfRides = heightLimitsOfRides;
    }

    public int rideableCount(int height) {
        return (int) heightLimitsOfRides.stream()
                .filter(limit -> height >= limit)
                .count();
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

import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideLimitsAndHeights")
    @DisplayName("놀이 기구의 키 제한과 키가 주어지면 탈 수 있는 놀이기구의 숫자를 반환한다.")
    void amusementParkReturnsRideableCountsGivenHeight(List<Integer> heightLimitsOfRides, int height, int expected) {
        AmusementPark sut = new AmusementPark(heightLimitsOfRides);

        int actual = sut.rideableCount(height);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideLimitsAndHeights() {
        return Stream.of(
                Arguments.of(List.of(100), 100, 1),
                Arguments.of(List.of(100, 100), 100, 2),
                Arguments.of(List.of(100, 101), 100, 1),
                Arguments.of(List.of(200, 90, 100, 123, 120, 169), 120, 3)
        );
    }
}

class AmusementPark {

    private final List<Integer> heightLimitsOfRides;

    public AmusementPark(List<Integer> heightLimitsOfRides) {
        this.heightLimitsOfRides = heightLimitsOfRides;
    }

    public int rideableCount(int height) {
        return (int) heightLimitsOfRides.stream()
                .filter(limit -> height >= limit)
                .count();
    }
}
~~~
