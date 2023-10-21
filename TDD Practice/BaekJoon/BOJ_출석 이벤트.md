# 출석 이벤트 (Bronze 4)

### 문제 설명

쇼핑몰에서 30일간 출석 이벤트를 진행한다. 쇼핑몰의 사이트를 방문하면 1일 1회 출석 도장을 받을 수 있고, 출석 도장을 여러 개 모아서 할인 쿠폰으로 교환할 수 있다.

출석 도장의 개수에 따라 교환할 수 있는 할인 쿠폰의 종류가 달라진다.

* 출석 도장 5개   → 500원 할인 쿠폰
* 출석 도장 10개 → 10% 할인 쿠폰
* 출석 도장 15개 → 2,000원 할인 쿠폰
* 출석 도장 20개 → 25% 할인 쿠폰

경태가 모은 출석 도장의 개수와 구매할 물건의 가격이 주어졌을 때, 경태가 지불해야 하는 최소 금액을 구하시오. 단, 할인 쿠폰은 최대 하나만 적용 가능하다. 할인 금액이 물건의 가격보다 더 큰 경우 지불해야 하는 금액은 0원이다.

출처 : https://www.acmicpc.net/problem/25704

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int countOfStamps = scanner.nextInt();
        int price = scanner.nextInt();
        Stamp stamp = new Stamp(countOfStamps, price);
        System.out.println(stamp.getPrice());
    }
}

class Stamp {

    private final int count;
    private final int price;

    public Stamp(int count, int price) {
        this.count = count;
        this.price = price;
    }

    public int getPrice() {
        Coupons coupons = new Coupons(count);
        return getMinimumPrice(coupons.getCoupons());
    }

    private int getMinimumPrice(List<Coupon> coupons) {
        return coupons.stream()
                .map(coupon -> coupon.getDiscountPrice(price))
                .reduce(price, Integer::min);
    }
}

class Coupons {

    private final int countOfStamps;

    public Coupons(int countOfStamps) {
        this.countOfStamps = countOfStamps;
    }

    public List<Coupon> getCoupons() {
        List<Coupon> coupons = new ArrayList<>();
        if (countOfStamps >= 20) {
            coupons.add(new PercentDiscountCoupon(25));
        }
        if (countOfStamps >= 15) {
            coupons.add(new AmountDiscountCoupon(2000));
        }
        if (countOfStamps >= 10) {
            coupons.add(new PercentDiscountCoupon(10));
        }
        if (countOfStamps >= 5) {
            coupons.add(new AmountDiscountCoupon(500));
        }
        return coupons;
    }
}

interface Coupon {
    int getDiscountPrice(int price);
}

class AmountDiscountCoupon implements Coupon {

    private static final int MINIMUM_PRICE = 0;

    private final int discountAmount;

    public AmountDiscountCoupon(int discountAmount) {
        this.discountAmount = discountAmount;
    }

    @Override
    public int getDiscountPrice(int price) {
        return Math.max(price - discountAmount, MINIMUM_PRICE);
    }
}

class PercentDiscountCoupon implements Coupon {

    private static final int PERCENT = 100;

    private final int discountPercent;

    public PercentDiscountCoupon(int discountPercent) {
        this.discountPercent = discountPercent;
    }

    @Override
    public int getDiscountPrice(int price) {
        return price - (price * discountPercent / PERCENT);
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

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideStampAndPrice")
    @DisplayName("물건을 구매할 가격을 반환한다.")
    void stampCalculatesPrice(int countOfStamps, int price, int expected) {
        Stamp sut = new Stamp(countOfStamps, price);

        int actual = sut.getPrice();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideStampAndPrice() {
        return Stream.of(
                Arguments.of(5, 5000, 4500),
                Arguments.of(5, 10000, 9500),
                Arguments.of(5, 100, 0),
                Arguments.of(10, 10000, 9000),
                Arguments.of(10, 1000, 500),
                Arguments.of(15, 3000, 1000),
                Arguments.of(20, 50000, 37500),
                Arguments.of(12, 50000, 45000),
                Arguments.of(23, 3000, 1000),
                Arguments.of(4, 3000, 3000)
        );
    }
}

class Stamp {

    private final int count;
    private final int price;

    public Stamp(int count, int price) {
        this.count = count;
        this.price = price;
    }

    public int getPrice() {
        Coupons coupons = new Coupons(count);
        return getMinimumPrice(coupons.getCoupons());
    }

    private int getMinimumPrice(List<Coupon> coupons) {
        return coupons.stream()
                .map(coupon -> coupon.getDiscountPrice(price))
                .reduce(price, Integer::min);
    }
}

class Coupons {

    private final int countOfStamps;

    public Coupons(int countOfStamps) {
        this.countOfStamps = countOfStamps;
    }

    public List<Coupon> getCoupons() {
        List<Coupon> coupons = new ArrayList<>();
        if (countOfStamps >= 20) {
            coupons.add(new PercentDiscountCoupon(25));
        }
        if (countOfStamps >= 15) {
            coupons.add(new AmountDiscountCoupon(2000));
        }
        if (countOfStamps >= 10) {
            coupons.add(new PercentDiscountCoupon(10));
        }
        if (countOfStamps >= 5) {
            coupons.add(new AmountDiscountCoupon(500));
        }
        return coupons;
    }
}

interface Coupon {
    int getDiscountPrice(int price);
}

class AmountDiscountCoupon implements Coupon {

    private static final int MINIMUM_PRICE = 0;

    private final int discountAmount;

    public AmountDiscountCoupon(int discountAmount) {
        this.discountAmount = discountAmount;
    }

    @Override
    public int getDiscountPrice(int price) {
        return Math.max(price - discountAmount, MINIMUM_PRICE);
    }
}

class PercentDiscountCoupon implements Coupon {

    private static final int PERCENT = 100;

    private final int discountPercent;

    public PercentDiscountCoupon(int discountPercent) {
        this.discountPercent = discountPercent;
    }

    @Override
    public int getDiscountPrice(int price) {
        return price - (price * discountPercent / PERCENT);
    }
}
~~~
