# 과자 (Bronze 4)

### 문제 설명

동수는 제과점에 과자를 사러 가는데 현재 가진 돈이 모자랄 경우 부모님께 모자란 돈을 받으려고 한다. 과자 한 개의 가격이 K, 사려고 하는 과자의 개수가 N이고, 현재 가진 돈의 액수를 M이라 할 때 여러분은 동수가 부모님께 받아야 하는 모자란 돈을 계산하려고 한다. 

예를 들어, 과자 한 개의 가격이 30원, 사려고 하는 과자의 개수가 4개, 현재 동수가 가진 돈이 100원이라 할 때, 동수가 부모님께 받아야 하는 돈은 20원이다. 과자 한 개의 가격이 250원, 사려고 하는 과자의 개수가 2개, 현재 동수가 가진 돈이 140원이라 할 때, 동수가 부모님께 받아야 하는 돈은 360원이다. 과자 한 개의 가격이 20원, 사려고 하는 과자의 개수가 6개, 현재 동수가 가진 돈이 120원이라 할 때 동수가 부모님께 받아야 하는 돈은 0원이다. 과자 한 개의 가격이 20원, 사려고 하는 과자의 개수가 10개, 현재 동수가 가진 돈이 320원이라 할 때 동수가 부모님께 받아야 하는 돈은 역시 0원이다. 

과자 한 개의 가격, 사려고 하는 과자의 개수와 동수가 현재 가진 돈의 액수가 주어질 때 동수가 부모님께 받아야 하는 돈의 액수를 출력하는 프로그램을 작성하시오. 

출처 : https://www.acmicpc.net/problem/10156

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Cookies cookies = new Cookies(scanner.nextInt(), scanner.nextInt(), scanner.nextInt());
        System.out.println(cookies.getNeededMoney());
    }
}

class Cookies {

    private static final int MINIMUM_MONEY = 0;

    private final int priceOfCookie;
    private final int cookieStock;
    private final int currentMoney;

    public Cookies(int priceOfCookie, int cookieStock, int currentMoney) {
        this.priceOfCookie = priceOfCookie;
        this.cookieStock = cookieStock;
        this.currentMoney = currentMoney;
    }

    public int getNeededMoney() {
        return Math.max(getEnoughMoney(), MINIMUM_MONEY);
    }

    private int getEnoughMoney() {
        return getTotalPriceOfCookies() - currentMoney;
    }

    private int getTotalPriceOfCookies() {
        return priceOfCookie * cookieStock;
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
    @MethodSource("providePriceAndStockAndMoney")
    @DisplayName("과자를 사기 위해서 필요한 돈의 액수를 반환한다.")
    void cookiesCalculateNeededMoney(int priceOfCookie, int countOfCookie, int currentMoney, int expected) {
        Cookies sut = new Cookies(priceOfCookie, countOfCookie, currentMoney);

        int actual = sut.getNeededMoney();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> providePriceAndStockAndMoney() {
        return Stream.of(
                Arguments.of(1, 1, 0, 1),
                Arguments.of(1, 1, 1, 0),
                Arguments.of(1, 2, 1, 1),
                Arguments.of(10, 2, 100, 0),
                Arguments.of(300, 4, 1000, 200),
                Arguments.of(250, 2, 140, 360),
                Arguments.of(20, 6, 120, 0),
                Arguments.of(20, 10, 320, 0)
        );
    }
}

class Cookies {

    private static final int MINIMUM_MONEY = 0;

    private final int priceOfCookie;
    private final int cookieStock;
    private final int currentMoney;

    public Cookies(int priceOfCookie, int cookieStock, int currentMoney) {
        this.priceOfCookie = priceOfCookie;
        this.cookieStock = cookieStock;
        this.currentMoney = currentMoney;
    }

    public int getNeededMoney() {
        return Math.max(getEnoughMoney(), MINIMUM_MONEY);
    }

    private int getEnoughMoney() {
        return getTotalPriceOfCookies() - currentMoney;
    }

    private int getTotalPriceOfCookies() {
        return priceOfCookie * cookieStock;
    }
}
~~~
