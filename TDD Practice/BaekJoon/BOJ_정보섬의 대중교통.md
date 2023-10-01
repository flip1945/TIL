# 정보섬의 대중교통 (Bronze 5)

### 문제 설명

숭실대학교 정보과학관은 숭실대입구역으로부터 멀리 떨어져 있는 대신, 바로 앞에 숭실대별관앞이라는 명칭의 버스 정류소가 자리 잡고 있다.

학부 연구생 찬솔이는 야근을 마치고 대중교통을 이용해 집에 가려고 한다. 다행히 아슬아슬하게 막차가 끊기지 않은 상황인데, 구체적으로 $A$분 뒤에 숭실대별관앞 정류소에 집으로 가는 마지막 버스가 도착하고, $B$분 뒤에 숭실대입구역에 집으로 가는 마지막 열차가 도착한다.

찬솔이는 지금 버스 정류소에 서 있다. 그런데, 찬솔이는 지하철 역까지 걸어가서 지하철을 타는 것이 여기서 버스를 타는 것 보다 빠를 수도 있다는 사실을 알아차려 버렸다. 숭실대입구역의 지하철 승강장까지 걸어가는데는 $N$분이 걸린다. 버스와 지하철 중 더 먼저 탈 수 있는 것이 무엇인지 알려줘서 야근한 찬솔이의 피로를 회복시켜 주자.

단, 버스와 지하철이 도착한 뒤 탑승하는 데 걸리는 시간은 신경 쓰지 않고, 버스와 지하철 모두 도착한 직후에 승객을 태운 뒤 기다리지 않고 바로 떠난다. 또한 지하철 역에 도착하는 시간과 지하철 열차가 도착하는 시간이 정확히 같다면 지하철을 탈 수 있다.

출처 : https://www.acmicpc.net/problem/28113

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.BiPredicate;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int walkTimeForSubway = scanner.nextInt();
        int remainingTimeOfBus = scanner.nextInt();
        int remainingTimeOfSubway = scanner.nextInt();

        PathFinder pathFinder = new PathFinder(walkTimeForSubway, remainingTimeOfBus, remainingTimeOfSubway);
        System.out.println(pathFinder.findTransportation());
    }
}

class PathFinder {

    private final int walkTimeForSubway;
    private final int remainingTimeOfBus;
    private final int remainingTimeOfSubway;

    public PathFinder(int walkTimeForSubway, int remainingTimeOfBus, int remainingTimeOfSubway) {
        this.walkTimeForSubway = walkTimeForSubway;
        this.remainingTimeOfBus = remainingTimeOfBus;
        this.remainingTimeOfSubway = remainingTimeOfSubway;
    }

    public String findTransportation() {
        Transportation transportation = Transportation.of(remainingTimeOfBus, remainingTimeOfSubway);
        return transportation.getName();
    }
}

enum Transportation {
    BUS("Bus", (remainingTimeOfBus, remainingTimeOfSubway) -> remainingTimeOfBus < remainingTimeOfSubway),
    SUBWAY("Subway", (remainingTimeOfBus, remainingTimeOfSubway) -> remainingTimeOfBus > remainingTimeOfSubway),
    ANYTHING("Anything", Integer::equals);

    private final String name;
    private final BiPredicate<Integer, Integer> matchFunction;

    Transportation(String name, BiPredicate<Integer, Integer> matchFunction) {
        this.name = name;
        this.matchFunction = matchFunction;
    }

    public static Transportation of(int remainingTimeOfBus, int remainingTimeOfSubway) {
        for (Transportation transportation : values()) {
            if (transportation.matchFunction.test(remainingTimeOfBus, remainingTimeOfSubway)) {
                return transportation;
            }
        }
        throw new IllegalArgumentException();
    }

    public String getName() {
        return name;
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

import java.util.function.BiPredicate;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePathFindingInfo")
    @DisplayName("패스파인더는 가장 빨리 탑승할 수 있는 교통 수단을 파악한다.")
    void pathFinderFindFastestPublicTransportationToBoard(int walkTimeForSubway, int remainingTimeOfBus,
                                                          int remainingTimeOfSubway, String expected) {
        PathFinder sut = new PathFinder(walkTimeForSubway, remainingTimeOfBus, remainingTimeOfSubway);

        String actual = sut.findTransportation();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> providePathFindingInfo() {
        return Stream.of(
                Arguments.of(5, 10, 6, "Subway"),
                Arguments.of(1, 100, 1, "Subway"),
                Arguments.of(5, 3, 6, "Bus"),
                Arguments.of(10, 5, 15, "Bus"),
                Arguments.of(5, 6, 6, "Anything"),
                Arguments.of(1, 1, 1, "Anything")
        );
    }
}

class PathFinder {

    private final int walkTimeForSubway;
    private final int remainingTimeOfBus;
    private final int remainingTimeOfSubway;

    public PathFinder(int walkTimeForSubway, int remainingTimeOfBus, int remainingTimeOfSubway) {
        this.walkTimeForSubway = walkTimeForSubway;
        this.remainingTimeOfBus = remainingTimeOfBus;
        this.remainingTimeOfSubway = remainingTimeOfSubway;
    }

    public String findTransportation() {
        Transportation transportation = Transportation.of(remainingTimeOfBus, remainingTimeOfSubway);
        return transportation.getName();
    }
}

enum Transportation {
    BUS("Bus", (remainingTimeOfBus, remainingTimeOfSubway) -> remainingTimeOfBus < remainingTimeOfSubway),
    SUBWAY("Subway", (remainingTimeOfBus, remainingTimeOfSubway) -> remainingTimeOfBus > remainingTimeOfSubway),
    ANYTHING("Anything", Integer::equals);

    private final String name;
    private final BiPredicate<Integer, Integer> matchFunction;

    Transportation(String name, BiPredicate<Integer, Integer> matchFunction) {
        this.name = name;
        this.matchFunction = matchFunction;
    }

    public static Transportation of(int remainingTimeOfBus, int remainingTimeOfSubway) {
        for (Transportation transportation : values()) {
            if (transportation.matchFunction.test(remainingTimeOfBus, remainingTimeOfSubway)) {
                return transportation;
            }
        }
        throw new IllegalArgumentException();
    }

    public String getName() {
        return name;
    }
}
~~~
