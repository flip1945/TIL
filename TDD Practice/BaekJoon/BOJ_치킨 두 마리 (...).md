# 치킨 두 마리 (...) (Bronze 4)

### 문제 설명

슬프게도, 2017 선린 봄맞이 교내대회의 상품 비용은 욱제의 통장에서 충당된다. 욱제의 마음을 아는지 모르는지, 참가자들이 1등 상품으로 치킨을 무려 두 마리(...)나 달라고 조르고 있다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/14489/1.png">

욱제에게는 두 개의 통장이 있다. 두 통장의 잔고와 치킨 한 마리의 가격이 주어질 때, 욱제가 치킨 두 마리(...)를 살 수 있는지 알아보자.

출처 : https://www.acmicpc.net/problem/14489

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        TwoChicken twoChicken = new TwoChicken(scanner.nextInt(), scanner.nextInt(), scanner.nextInt());
        System.out.println(twoChicken.getRemainingBalanceAfterBuyingTwoChicken());
    }
}

class TwoChicken {

    private final int firstBalance;
    private final int secondBalance;
    private final int priceOfChicken;

    public TwoChicken(int firstBalance, int secondBalance, int priceOfChicken) {
        this.firstBalance = firstBalance;
        this.secondBalance = secondBalance;
        this.priceOfChicken = priceOfChicken;
    }

    public int getRemainingBalanceAfterBuyingTwoChicken() {
        if (getSumOfBalances() >= priceOfTwoChicken()) {
            return getSumOfBalances() - priceOfTwoChicken();
        }
        return getSumOfBalances();
    }

    private int getSumOfBalances() {
        return firstBalance + secondBalance;
    }

    private int priceOfTwoChicken() {
        return priceOfChicken * 2;
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

import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideTwoBalancesAndPriceOfChicken")
    @DisplayName("치킨 두 마리를 사고 남은 잔고를 반환해야 한다.")
    void shouldReturnRemainingBalanceAfterBuyingTwoChicken(int firstBalance, int secondBalance, int priceOfChicken, int expected) {
        TwoChicken sut = new TwoChicken(firstBalance, secondBalance, priceOfChicken);

        int actual = sut.getRemainingBalanceAfterBuyingTwoChicken();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideTwoBalancesAndPriceOfChicken() {
        return Stream.of(
                Arguments.of(10, 10, 100, 20),
                Arguments.of(10, 30, 100, 40),
                Arguments.of(87, 31, 20000, 118),
                Arguments.of(15000, 6000, 5000, 11000),
                Arguments.of(1_000_000_000, 1_000_000_000, 1_000_000_001, 2_000_000_000),
                Arguments.of(5, 5, 5, 0),
                Arguments.of(10, 20, 10, 10)
        );
    }
}

class TwoChicken {

    private final int firstBalance;
    private final int secondBalance;
    private final int priceOfChicken;

    public TwoChicken(int firstBalance, int secondBalance, int priceOfChicken) {
        this.firstBalance = firstBalance;
        this.secondBalance = secondBalance;
        this.priceOfChicken = priceOfChicken;
    }

    public int getRemainingBalanceAfterBuyingTwoChicken() {
        if (getSumOfBalances() >= priceOfTwoChicken()) {
            return getSumOfBalances() - priceOfTwoChicken();
        }
        return getSumOfBalances();
    }

    private int getSumOfBalances() {
        return firstBalance + secondBalance;
    }

    private int priceOfTwoChicken() {
        return priceOfChicken * 2;
    }
}
~~~
