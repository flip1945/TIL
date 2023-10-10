# 특식 배부 (Bronze 4)

### 문제 설명

설날을 맞아 부대원들을 위해 특식으로 치킨을 주문했다. 후라이드 치킨, 간장치킨, 양념치킨을 각각 $N$마리씩 주문했고, $1$인당 치킨을 한 마리씩 배부하고자 한다.

최대한 많은 부대원에게 본인이 선호하는 종류의 치킨을 배부해주기 위해 으뜸병사는 부대원들의 치킨 종류 선호도를 조사했고, 세 가지 치킨 중 후라이드 치킨, 간장치킨, 양념치킨을 가장 선호하는 인원의 수는 각각 $A$명, $B$명, $C$명이라는 것을 알아냈다. 이때, 모든 부대원은 각자 한 종류의 치킨만 골라 답했다.

본인이 가장 선호하는 종류의 치킨을 받을 수 있는 인원수의 최댓값을 구하여라.

출처 : https://www.acmicpc.net/problem/27110

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Chickens chickens = new Chickens(scanner.nextInt(), scanner.nextInt(), scanner.nextInt(), scanner.nextInt());
        System.out.println(chickens.getMaxPeople());
    }
}

class Chickens {

    private final int countOfChickensOfEachType;
    private final int friedChicken;
    private final int soySauceChicken;
    private final int seasonedChicken;

    public Chickens(int countOfChickensOfEachType, int friedChicken, int soySauceChicken, int seasonedChicken) {
        this.countOfChickensOfEachType = countOfChickensOfEachType;
        this.friedChicken = friedChicken;
        this.soySauceChicken = soySauceChicken;
        this.seasonedChicken = seasonedChicken;
    }

    public int getMaxPeople() {
        int countOfFriedChicken = getCountOfChicken(friedChicken);
        int countOfSoySauceChicken = getCountOfChicken(soySauceChicken);
        int countOfSeasonedChicken = getCountOfChicken(seasonedChicken);
        return countOfFriedChicken + countOfSoySauceChicken + countOfSeasonedChicken;
    }

    private int getCountOfChicken(int countOfChicken) {
        return Math.min(countOfChickensOfEachType, countOfChicken);
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
    @MethodSource("provideChickens")
    @DisplayName("각 치킨의 마리수와 각 치킨을 원하는 사람의 수가 주어지면 원하는 치킨을 받는 최대 인원수를 반환한다.")
    void testChicken(int countOfChickens, int friedChicken, int soySauceChicken, int seasonedChicken, int expected) {
        Chickens sut = new Chickens(countOfChickens, friedChicken, soySauceChicken, seasonedChicken);

        int actual = sut.getMaxPeople();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideChickens() {
        return Stream.of(
                Arguments.of(1, 1, 1, 1, 3),
                Arguments.of(2, 2, 2, 2, 6),
                Arguments.of(2, 1, 2, 2, 5),
                Arguments.of(5, 1, 7, 6, 11)
        );
    }
}

class Chickens {

    private final int countOfChickensOfEachType;
    private final int friedChicken;
    private final int soySauceChicken;
    private final int seasonedChicken;

    public Chickens(int countOfChickensOfEachType, int friedChicken, int soySauceChicken, int seasonedChicken) {
        this.countOfChickensOfEachType = countOfChickensOfEachType;
        this.friedChicken = friedChicken;
        this.soySauceChicken = soySauceChicken;
        this.seasonedChicken = seasonedChicken;
    }

    public int getMaxPeople() {
        int countOfFriedChicken = getCountOfChicken(friedChicken);
        int countOfSoySauceChicken = getCountOfChicken(soySauceChicken);
        int countOfSeasonedChicken = getCountOfChicken(seasonedChicken);
        return countOfFriedChicken + countOfSoySauceChicken + countOfSeasonedChicken;
    }

    private int getCountOfChicken(int countOfChicken) {
        return Math.min(countOfChickensOfEachType, countOfChicken);
    }
}
~~~
