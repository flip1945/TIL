# Final Price (Bronze 4)

### 문제 설명

Neverland has recently experienced a rapid rise in the inflation rate. The value of money is continuously decreasing, and citizens’ purchasing power is lowered daily. The government is trying to control the inflation rate by testing various methods, such as reducing the amount of money in the economy by increasing interest rates and promoting investment, even in purchasing cars to be delivered in an unforeseeable future.

Despite these efforts, the inflation rate is still above 50%, and the prices are jumping up and down drastically every now and then. The Statistical Center of Neverland provides detailed information on the inflation rate and the average price change over time for a basket of goods commonly purchased by households. However, it is hard to calculate the actual price of a specific good at a given point in time using that information.

The ICPC (International Center for Price Changes) is asking you to help the citizens of Neverland to calculate the prices for their desired goods after a specific period of time. The information provided to you is the initial price of a good at the start of a time period, and the daily price change for that good over the observed time period.

출처 : https://www.acmicpc.net/problem/28224

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
        FinalPrice finalPrice = new FinalPrice(inputPrices(scanner, n));

        System.out.println(finalPrice.getFinalPrice());
    }

    private static List<Integer> inputPrices(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class FinalPrice {

    private final List<Integer> prices;

    public FinalPrice(List<Integer> prices) {
        this.prices = prices;
    }

    public int getFinalPrice() {
        return prices.stream()
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

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePrices")
    @DisplayName("가격들이 주어지면 최종 가격을 구한다.")
    void finalPriceReturnsFinalPriceGivenPrices(List<Integer> prices, int expected) {
        FinalPrice sut = new FinalPrice(prices);

        int actual = sut.getFinalPrice();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> providePrices() {
        return Stream.of(
                Arguments.of(List.of(1, 2), 3),
                Arguments.of(List.of(4, 5, 6), 15),
                Arguments.of(List.of(11, 68), 79),
                Arguments.of(List.of(110, -5, 0, 27), 132)
        );
    }
}

class FinalPrice {

    private final List<Integer> prices;

    public FinalPrice(List<Integer> prices) {
        this.prices = prices;
    }

    public int getFinalPrice() {
        return prices.stream()
                .mapToInt(Integer::intValue)
                .sum();
    }
}
~~~
