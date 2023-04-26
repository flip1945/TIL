# 오르막길 (Bronze 1)

### 문제 설명

상근이는 자전거를 타고 등교한다. 자전거 길은 오르막길, 내리막길, 평지로 이루어져 있다. 상근이는 개강 첫 날 자전거를 타고 가면서 일정 거리마다 높이를 측정했다. 상근이는 가장 큰 오르막길의 크기를 구하려고 한다.

측정한 높이는 길이가 N인 수열로 나타낼 수 있다. 여기서 오르막길은 적어도 2개의 수로 이루어진 높이가 증가하는 부분 수열이다. 오르막길의 크기는 부분 수열의 첫 번째 숫자와 마지막 숫자의 차이이다.

예를 들어, 높이가 다음과 같은 길이 있다고 하자. 12 3 5 7 10 6 1 11. 이 길에는 2 개의 오르막길이 있다. 밑 줄로 표시된 부분 수열이 오르막길이다. 첫 번째 오르막길의 크기는 7이고, 두 번째 오르막길의 크기는 10이다. 높이가 12와 6인 곳은 오르막길에 속하지 않는다.

가장 큰 오르막길을 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/2846

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<Integer> roads = IntStream.range(0, n)
                .map(i -> scanner.nextInt())
                .boxed()
                .collect(Collectors.toList());
        System.out.println(getMaxAscentValue(roads));
    }

    static int getMaxAscentValue(List<Integer> roads) {
        int result = 0;
        Ascent ascent = new Ascent(roads.get(0));
        for (int i = 1; i < roads.size(); i++) {
            ascent.nextAscent(roads.get(i));
            result = getMaxResult(result, ascent);
        }
        return result;
    }

    static int getMaxResult(int result, Ascent ascent) {
        return Math.max(result, ascent.getAscentValue());
    }
}

class Ascent {
    private final List<Integer> ascent = new ArrayList<>();

    public Ascent(int initialRoad) {
        this.ascent.add(initialRoad);
    }

    public void nextAscent(int nextRoad) {
        if (nextRoad <= ascent.get(getMaxAscent())) {
            ascent.clear();
        }
        ascent.add(nextRoad);
    }

    private int getMaxAscent() {
        return ascent.size() - 1;
    }

    public int getAscentValue() {
        return ascent.get(getMaxAscent()) - ascent.get(0);
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

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideRoads")
    @DisplayName("가장 큰 오르막길을 반환해야 한다.")
    void getMaxAscentValueTest(List<Integer> roads, int expected) {
        int actual = getMaxAscentValue(roads);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideRoads() {
        return Stream.of(
                Arguments.of(List.of(1, 2, 3, 4, 5), 4),
                Arguments.of(List.of(1, 2, 3, 4), 3),
                Arguments.of(List.of(1, 2, 3, 1), 2),
                Arguments.of(List.of(1, 2, 1, 4, 6), 5),
                Arguments.of(List.of(12, 20, 1, 3, 4, 4, 11, 1), 8),
                Arguments.of(List.of(3, 2, 1), 0),
                Arguments.of(List.of(10, 8, 8, 6, 4, 3), 0)
        );
    }

    private int getMaxAscentValue(List<Integer> roads) {
        int result = 0;
        Ascent ascent = new Ascent(roads.get(0));
        for (int i = 1; i < roads.size(); i++) {
            ascent.nextAscent(roads.get(i));
            result = getMaxResult(result, ascent);
        }
        return result;
    }

    private int getMaxResult(int result, Ascent ascent) {
        return Math.max(result, ascent.getAscentValue());
    }
}

class Ascent {
    private final List<Integer> ascent = new ArrayList<>();

    public Ascent(int initialRoad) {
        this.ascent.add(initialRoad);
    }

    public void nextAscent(int nextRoad) {
        if (nextRoad <= ascent.get(getMaxAscent())) {
            ascent.clear();
        }
        ascent.add(nextRoad);
    }

    private int getMaxAscent() {
        return ascent.size() - 1;
    }

    public int getAscentValue() {
        return ascent.get(getMaxAscent()) - ascent.get(0);
    }
}
~~~
