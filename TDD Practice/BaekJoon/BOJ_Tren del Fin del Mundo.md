# Tren del Fin del Mundo (Bronze 4)

### 문제 설명

Southern Fuegian Railway는 세상에서 가장 남쪽에 있는 철도이다.

Southern Fuegian Railway는 $x$축의 양의 방향을 동쪽으로 하는 $2$차원 좌표평면으로 나타내어진다.

Southern Fuegian Railway는 $N$개의 역과 역 사이를 잇는 $N-1$개의 철로로 구성되어 있다. $i$번째 역은 $(x_i,y_i)$에 있으며, $j$번째 철로는 $j$번 역과 $j+1$번 역 사이를 잇는 선분이다. $(1 \le i \le N;$ $1 \le j \le N-1)$ 

Southern Fuegian Railway를 보러 간 선아는 세상에서 가장 남쪽에 있는 철도가 지나는 가장 남쪽 점이 어디일지 궁금해졌다.

출처 : https://www.acmicpc.net/problem/30876

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> point = inputPoints(scanner, n);

        Stations stations = new Stations(point);
        System.out.println(stations.southernmostStation());
    }

    private static List<String> inputPoints(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Stations {

    private final List<StationPoint> points;

    public Stations(List<String> points) {
        this.points = initStationPoints(points);
    }

    private List<StationPoint> initStationPoints(List<String> points) {
        return points.stream()
                .map(StationPoint::from)
                .collect(Collectors.toList());
    }

    public String southernmostStation() {
        return points.stream()
                .sorted()
                .findFirst()
                .orElseThrow(IllegalArgumentException::new)
                .toString();
    }
}

class StationPoint implements Comparable<StationPoint> {
    private static final String DELIMITER = " ";
    private static final int X_INDEX = 0;
    private static final int Y_INDEX = 1;

    private final int x;
    private final int y;

    public StationPoint(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public static StationPoint from(String pointString) {
        String[] splitPoint = pointString.split(DELIMITER);
        return new StationPoint(Integer.parseInt(splitPoint[X_INDEX]), Integer.parseInt(splitPoint[Y_INDEX]));
    }

    @Override
    public int compareTo(StationPoint o) {
        return Integer.compare(this.y, o.y);
    }

    @Override
    public String toString() {
        return x + DELIMITER + y;
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
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePoints")
    @DisplayName("역의 좌표가 주어지면 가장 남쪽에 있는 좌표를 반환한다.")
    void stationsReturnsTheSouthernmostStationPoint(List<String> points, String expected) {
        Stations sut = new Stations(points);

        String actual = sut.southernmostStation();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> providePoints() {
        return Stream.of(
                Arguments.of(List.of("1 1", "2 2"), "1 1"),
                Arguments.of(List.of("3 1", "2 2"), "3 1"),
                Arguments.of(List.of("10 5", "6 -3", "3 2", "4 2"), "6 -3")
        );
    }
}

class Stations {

    private final List<StationPoint> points;

    public Stations(List<String> points) {
        this.points = initStationPoints(points);
    }

    private List<StationPoint> initStationPoints(List<String> points) {
        return points.stream()
                .map(StationPoint::from)
                .collect(Collectors.toList());
    }

    public String southernmostStation() {
        return points.stream()
                .sorted()
                .findFirst()
                .orElseThrow(IllegalArgumentException::new)
                .toString();
    }
}

class StationPoint implements Comparable<StationPoint> {
    private static final String DELIMITER = " ";
    private static final int X_INDEX = 0;
    private static final int Y_INDEX = 1;

    private final int x;
    private final int y;

    public StationPoint(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public static StationPoint from(String pointString) {
        String[] splitPoint = pointString.split(DELIMITER);
        return new StationPoint(Integer.parseInt(splitPoint[X_INDEX]), Integer.parseInt(splitPoint[Y_INDEX]));
    }

    @Override
    public int compareTo(StationPoint o) {
        return Integer.compare(this.y, o.y);
    }

    @Override
    public String toString() {
        return x + DELIMITER + y;
    }
}
~~~
