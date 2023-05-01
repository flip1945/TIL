# 양념 반 후라이드 반 (Bronze 2)

### 문제 설명

현진 치킨에서 판매하는 치킨은 양념 치킨, 후라이드 치킨, 반반 치킨으로 총 세 종류이다. 반반 치킨은 절반은 양념 치킨, 절반은 후라이드 치킨으로 이루어져있다. 양념 치킨 한 마리의 가격은 A원, 후라이드 치킨 한 마리의 가격은 B원, 반반 치킨 한 마리의 가격은 C원이다.

상도는 오늘 파티를 위해 양념 치킨 최소 X마리, 후라이드 치킨 최소 Y마리를 구매하려고 한다. 반반 치킨을 두 마리 구입해 양념 치킨 하나와 후라이드 치킨 하나를 만드는 방법도 가능하다. 상도가 치킨을 구매하는 금액의 최솟값을 구해보자.

출처 : https://www.acmicpc.net/problem/16917

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int friedChickenPrice = scanner.nextInt();
        int seasonedChickenPrice = scanner.nextInt();
        int mixedChickenPrice = scanner.nextInt();
        int countOfFriedChicken = scanner.nextInt();
        int countOfSeasonedChicken = scanner.nextInt();
        ChickenStore chickenStore = new ChickenStore(friedChickenPrice, seasonedChickenPrice, mixedChickenPrice);

        System.out.println(chickenStore.getMinChickenPrice(countOfFriedChicken, countOfSeasonedChicken));
    }
}

class ChickenStore {
    private final int friedChickenPrice;
    private final int seasonedChickenPrice;
    private final int mixedChickenPrice;
    private int countOfFriedChicken;
    private int countOfSeasonedChicken;

    public ChickenStore(int friedChickenPrice, int seasonedChickenPrice, int mixedChickenPrice) {
        this.friedChickenPrice = friedChickenPrice;
        this.seasonedChickenPrice = seasonedChickenPrice;
        this.mixedChickenPrice = mixedChickenPrice;
    }

    public int getMinChickenPrice(int countOfFriedChicken, int countOfSeasonedChicken) {
        this.countOfFriedChicken = countOfFriedChicken;
        this.countOfSeasonedChicken = countOfSeasonedChicken;
        int commonChickenPrice = getCommonChickenPrice();
        int totalFriedChickenPrice = getTotalChickenPrice(this.countOfFriedChicken, friedChickenPrice);
        int totalSeasonedChickenPrice = getTotalChickenPrice(this.countOfSeasonedChicken, seasonedChickenPrice);
        return commonChickenPrice + totalFriedChickenPrice + totalSeasonedChickenPrice;
    }

    private int getCommonChickenPrice() {
        int result = 0;
        if (isCheaperTwoMixedChicken(friedChickenPrice + seasonedChickenPrice)) {
            int commonChickenCount = Math.min(this.countOfFriedChicken, this.countOfSeasonedChicken);
            result += mixedChickenPrice * 2 * commonChickenCount;
            this.countOfFriedChicken -= commonChickenCount;
            this.countOfSeasonedChicken -= commonChickenCount;
        }
        return result;
    }

    private int getTotalChickenPrice(int countOfChicken, int chickenPrice) {
        if (isCheaperTwoMixedChicken(chickenPrice)) {
            return mixedChickenPrice * 2 * countOfChicken;
        }
        return chickenPrice * countOfChicken;
    }

    private boolean isCheaperTwoMixedChicken(int chickenPrice) {
        return chickenPrice > mixedChickenPrice * 2;
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
    @MethodSource("provideChickens")
    @DisplayName("치킨을 구매하는 금액의 최소값을 반환한다.")
    void getMinChickenPriceTest(int friedChickenPrice, int seasonedChickenPrice, int mixedChickenPrice,
                                int countOfFriedChicken, int countOfSeasonedChicken, int expected) {
        ChickenStore sut = new ChickenStore(friedChickenPrice, seasonedChickenPrice, mixedChickenPrice);

        int actual = sut.getMinChickenPrice(countOfFriedChicken, countOfSeasonedChicken);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideChickens() {
        return Stream.of(
                Arguments.of(100, 200, 1000, 1, 1, 300)
                , Arguments.of(200, 200, 1000, 1, 1, 400)
                , Arguments.of(100, 200, 1000, 10, 10, 3000)
                , Arguments.of(100, 200, 50, 1, 1, 100)
                , Arguments.of(1500, 2000, 1600, 3, 2, 7900)
                , Arguments.of(1500, 2000, 1900, 3, 2, 8500)
                , Arguments.of(1500, 2000, 500, 90000, 100000, 100000000)
        );
    }
}

class ChickenStore {
    private final int friedChickenPrice;
    private final int seasonedChickenPrice;
    private final int mixedChickenPrice;
    private int countOfFriedChicken;
    private int countOfSeasonedChicken;

    public ChickenStore(int friedChickenPrice, int seasonedChickenPrice, int mixedChickenPrice) {
        this.friedChickenPrice = friedChickenPrice;
        this.seasonedChickenPrice = seasonedChickenPrice;
        this.mixedChickenPrice = mixedChickenPrice;
    }

    public int getMinChickenPrice(int countOfFriedChicken, int countOfSeasonedChicken) {
        this.countOfFriedChicken = countOfFriedChicken;
        this.countOfSeasonedChicken = countOfSeasonedChicken;
        int commonChickenPrice = getCommonChickenPrice();
        int totalFriedChickenPrice = getTotalChickenPrice(this.countOfFriedChicken, friedChickenPrice);
        int totalSeasonedChickenPrice = getTotalChickenPrice(this.countOfSeasonedChicken, seasonedChickenPrice);
        return commonChickenPrice + totalFriedChickenPrice + totalSeasonedChickenPrice;
    }

    private int getCommonChickenPrice() {
        int result = 0;
        if (isCheaperTwoMixedChicken(friedChickenPrice + seasonedChickenPrice)) {
            int commonChickenCount = Math.min(this.countOfFriedChicken, this.countOfSeasonedChicken);
            result += mixedChickenPrice * 2 * commonChickenCount;
            this.countOfFriedChicken -= commonChickenCount;
            this.countOfSeasonedChicken -= commonChickenCount;
        }
        return result;
    }

    private int getTotalChickenPrice(int countOfChicken, int chickenPrice) {
        if (isCheaperTwoMixedChicken(chickenPrice)) {
            return mixedChickenPrice * 2 * countOfChicken;
        }
        return chickenPrice * countOfChicken;
    }

    private boolean isCheaperTwoMixedChicken(int chickenPrice) {
        return chickenPrice > mixedChickenPrice * 2;
    }
}
~~~
