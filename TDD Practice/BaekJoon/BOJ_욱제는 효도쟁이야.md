# 욱제는 효도쟁이야!! (Bronze 2)

### 문제 설명

욱제는 KOI를 망친 기념으로 부모님과 함께 코드게이트 섬으로 여행을 떠났다. 코드게이트 섬에는 오징어로 유명한 준오마을(심술쟁이 해커 임준오 아님), 밥으로 유명한 재훈마을, 영중마을 등 많은 관광지들이 있다. 욱제는 부모님을 모시고 코드게이트 섬을 관광하려고 한다.

코드게이트 섬은 해안가를 따라 원형으로 마을들이 위치해있다. 임의의 A마을에서 임의의 B마을로 가기 위해서는 왼쪽 또는 오른쪽 도로를 통해 해안가를 따라 섬을 돌아야 한다. 섬을 빙빙 도는 원형의 길 외에 다른 길은 존재하지 않는다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/14487/1.png" width=300px>

각 마을에서 마을까지의 이동비용이 주어질 때, 욱제가 최소한의 이동비용으로 부모님을 모시고 섬의 모든 마을을 관광하려면 얼마의 이동비용을 준비해야하는지 알려주자.

출처 : https://www.acmicpc.net/problem/14487

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<Integer> travelCosts = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());

        System.out.println(new TravelCostCalculator(travelCosts).calculateMinTravelCost());
    }
}

class TravelCostCalculator {
    private final List<Integer> travelCosts;

    public TravelCostCalculator(List<Integer> travelCosts) {
        this.travelCosts = travelCosts;
    }

    public int calculateMinTravelCost() {
        return getTotalCost() - getLargestCost();
    }

    private int getLargestCost() {
        return travelCosts.stream()
                .mapToInt(Integer::intValue)
                .max()
                .orElse(0);
    }

    private int getTotalCost() {
        return travelCosts.stream()
                .mapToInt(Integer::intValue)
                .sum();
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
    @MethodSource("provideTravelCosts")
    @DisplayName("모든 마을을 관광하기 위한 최소 이동 비용을 반환한다.")
    void getMinTravelCostTest(List<Integer> travelCosts, int expected) {
        TravelCostCalculator sut = new TravelCostCalculator(travelCosts);

        int actual = sut.calculateMinTravelCost();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideTravelCosts() {
        return Stream.of(
                Arguments.of(List.of(1, 6, 5, 2, 4), 12)
                , Arguments.of(List.of(1, 6, 5, 2, 4, 8), 18)
                , Arguments.of(List.of(100, 100, 100, 101), 300)
                , Arguments.of(List.of(1, 1, 1), 2)
                , Arguments.of(List.of(1000, 1000, 1000), 2000)
        );
    }
}

class TravelCostCalculator {
    private final List<Integer> travelCosts;

    public TravelCostCalculator(List<Integer> travelCosts) {
        this.travelCosts = travelCosts;
    }

    public int calculateMinTravelCost() {
        return getTotalCost() - getLargestCost();
    }

    private int getLargestCost() {
        return travelCosts.stream()
                .mapToInt(Integer::intValue)
                .max()
                .orElse(0);
    }

    private int getTotalCost() {
        return travelCosts.stream()
                .mapToInt(Integer::intValue)
                .sum();
    }
}
~~~
