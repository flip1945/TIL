# 속도 위반 (Silver 5)

### 문제 설명

말썽꾸러기 연정이는 오늘도 태우의 자동차를 몰래타고 신나게 도로를 달리는 중이다.

도로는 정확히 100km 이고, 연정이는 무조건 도로의 끝까지 달려야한다.

도로의 각 구간에는 제한속도를 지정해 두었으나 쿨한 연정이는 속도 위반에는 개의치 않아 (더군다나 자신의 차도 아니므로) 자신이 원하는 속도로 달린다.

도로는 N개의 구간으로 나뉘며 각 구간에는 도로 길이와 해당 도로의 제한속도가 주어진다. 

도로 N개의 총 합은 100km 이며 각 구간별 도로 길이와 제한 속도는 모두 양의 정수이다. 예를 들어, N이 3이고 (40, 75), (50, 35), (10, 45) 이라면 

* 첫 구간의 도로 길이는 40km, 제한속도는 75km/h 
* 두 번째 구간의 도로 길이는 50km, 제한 속도는 35km/h 
* 세 번째 구간의 도로 길이는 10km, 제한 속도는 45km/h

연정이가 달린 도로 또한 M 개 구간으로 나뉘며 각 구간에는 도로 길이와 연정이가 달린 속도가 주어진다. 

M 개의 도로 총 합은 100km 이며 각 구간별 도로 길이와 달린 속도는 모두 양의 정수이다. 예를 들어 M 이 3이고 (40, 76), (20, 30), (40, 40) 이라면 

* 첫 구간에서 연정이가 달린 도로 길이는 40km, 달린 속도는 76km/h
* 두 번째 구간에서 달린 도로 길이는 20km, 달린 속도는 30km/h
* 세 번째 구간에서 달린 도로 길이는 40km, 달린 속도는 40km/h

연정이가 100km 도로를 달리는 동안 속도를 위반한 최댓값을 구하시오.

출처 : https://www.acmicpc.net/problem/11971

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        List<String> limits = IntStream.range(0, n).mapToObj(i -> scanner.nextLine()).collect(Collectors.toList());
        List<String> speeds = IntStream.range(0, m).mapToObj(i -> scanner.nextLine()).collect(Collectors.toList());

        SpeedingCalculator calculator = SpeedingCalculator.INSTANCE;
        System.out.println(calculator.maxSpeeding(limits, speeds));
    }
}

enum SpeedingCalculator {
    INSTANCE;

    public static final int ROAD_SIZE = 100;
    public static final int ROAD_INDEX = 0;
    public static final int SPEED_INDEX = 1;

    public int maxSpeeding(List<String> limits, List<String> speeds) {
        return calculateMaxSpeeding(getRoadsOfLimit(limits), getRoadsOfSpeed(speeds));
    }

    private int calculateMaxSpeeding(int[] roadOfLimits, int[] roadOfSpeed) {
        int result = 0;
        for (int i = 0; i < ROAD_SIZE; i++) {
            result = Math.max(result, roadOfSpeed[i] - roadOfLimits[i]);
        }
        return result;
    }

    private int[] getRoadsOfSpeed(List<String> speeds) {
        return getRoads(speeds);
    }

    private int[] getRoadsOfLimit(List<String> limits) {
        return getRoads(limits);
    }

    private int[] getRoads(List<String> speedInfos) {
        int[] roads = new int[ROAD_SIZE];
        int currentRoad = 0;
        for (String info : speedInfos) {
            String[] split = info.split(" ");
            int road = Integer.parseInt(split[ROAD_INDEX]);
            int speed = Integer.parseInt(split[SPEED_INDEX]);
            Arrays.fill(roads, currentRoad, currentRoad + road, speed);
            currentRoad += road;
        }
        return roads;
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

import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNames")
    @DisplayName("속도 위반한 최댓값을 반환한다.")
    void speedingTest(List<String> limits, List<String> speeds, int expected) {
        SpeedingCalculator sut = SpeedingCalculator.INSTANCE;

        int actual = sut.maxSpeeding(limits, speeds);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNames() {
        return Stream.of(
                Arguments.of(List.of("100 0"), List.of("10 1", "90 0"), 1),
                Arguments.of(List.of("100 10"), List.of("10 1", "90 0"), 0),
                Arguments.of(List.of("100 10"), List.of("10 1", "80 0", "10 100"), 90),
                Arguments.of(List.of("40 75", "50 35", "10 45"), List.of("40 76", "20 30", "40 40"), 5)
        );
    }
}

enum SpeedingCalculator {
    INSTANCE;

    public static final int ROAD_SIZE = 100;
    public static final int ROAD_INDEX = 0;
    public static final int SPEED_INDEX = 1;

    public int maxSpeeding(List<String> limits, List<String> speeds) {
        return calculateMaxSpeeding(getRoadsOfLimit(limits), getRoadsOfSpeed(speeds));
    }

    private int calculateMaxSpeeding(int[] roadOfLimits, int[] roadOfSpeed) {
        int result = 0;
        for (int i = 0; i < ROAD_SIZE; i++) {
            result = Math.max(result, roadOfSpeed[i] - roadOfLimits[i]);
        }
        return result;
    }

    private int[] getRoadsOfSpeed(List<String> speeds) {
        return getRoads(speeds);
    }

    private int[] getRoadsOfLimit(List<String> limits) {
        return getRoads(limits);
    }

    private int[] getRoads(List<String> speedInfos) {
        int[] roads = new int[ROAD_SIZE];
        int currentRoad = 0;
        for (String info : speedInfos) {
            String[] split = info.split(" ");
            int road = Integer.parseInt(split[ROAD_INDEX]);
            int speed = Integer.parseInt(split[SPEED_INDEX]);
            Arrays.fill(roads, currentRoad, currentRoad + road, speed);
            currentRoad += road;
        }
        return roads;
    }
}
~~~
