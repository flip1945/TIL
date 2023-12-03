# New Financial Year (Bronze 3)

### 문제 설명

Naomi has spent the past year tracking her doughnut sales. With so many different flavours, some are bound to sell more than others. In order to maximize her sales for the coming year, she keeps track of certain information on each flavor. She tracks exactly $N$ different flavours of doughnuts. For each flavour, she tracks $O$, the original price, $P$, the new price, and $C$, the relative change in price. The relative change in price is computed using the formula $C=100\% \cdot (P-O)/O$.

Unfortunately, during one of her late nights, while analyzing her clipboard of data, she spilled coffee over the entire section of the page that keeps track of the original price of each doughnut flavour.

Can you help Lesley recover her data, specifically $O$, the original price of each doughnut flavour?

출처 : https://www.acmicpc.net/problem/21105

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            Price price = new Price(scanner.nextDouble(), scanner.nextDouble());
            System.out.println(price.originalPrice());
        }
    }
}

class Price {

    private static final int PERCENT = 100;

    private final double newPrice;
    private final double changeRate;

    public Price(double newPrice, double changeRate) {
        this.newPrice = newPrice;
        this.changeRate = changeRate;
    }

    public double originalPrice() {
        return PERCENT * newPrice / (PERCENT + changeRate);
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
    @MethodSource("provideNewPriceAndChangeRate")
    @DisplayName("새 가격과 변화율이 주어지면 원래의 가격을 반환한다.")
    void priceReturnsOriginalPriceGivenNewPriceAndChangeRate(double newPrice, double changeRate, double expected) {
        Price sut = new Price(newPrice, changeRate);

        double actual = sut.originalPrice();

        assertEquals(expected, actual, 0.00001);
    }

    static Stream<Arguments> provideNewPriceAndChangeRate() {
        return Stream.of(
                Arguments.of(100.00, 10.50, 90.497737557),
                Arguments.of(50.00, -50.75, 101.522842640)
        );
    }
}

class Price {

    private static final int PERCENT = 100;

    private final double newPrice;
    private final double changeRate;

    public Price(double newPrice, double changeRate) {
        this.newPrice = newPrice;
        this.changeRate = changeRate;
    }

    public double originalPrice() {
        return PERCENT * newPrice / (PERCENT + changeRate);
    }
}
~~~
