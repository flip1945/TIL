# Pups (Bronze 5)

### 문제 설명

Congratulations, you adopted some little puppies! Now you just need to go grab food for them at the store. Your vet tells you how many pounds of food each pup will eat before your next trip to the store, so you just need to calculate the total amount of food that you will need to buy. You also know how much food costs per pound, so you just need to make sure that you bring the right amount of money to pay for the food. Write a program that, given the number of puppies, the amount of food per puppy, and the price per pound of food, calculates the amount of money that you will need to bring.

출처 : https://www.acmicpc.net/problem/26575

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            double numberOfDogs = scanner.nextDouble();
            double foodAmountPerPounds = scanner.nextDouble();
            double foodPricePerPounds = scanner.nextDouble();

            Feed feed = new Feed(numberOfDogs, foodAmountPerPounds, foodPricePerPounds);
            System.out.println(feed.totalPrice());
        }
    }
}

class Feed {

    private static final String MONEY_SIGN = "$";
    private static final String PRICE_FORMAT = "%.2f";

    private final double numberOfDogs;
    private final double foodAmountPerPounds;
    private final double foodPricePerPounds;

    public Feed(double numberOfDogs, double foodAmountPerPounds, double foodPricePerPounds) {
        this.numberOfDogs = numberOfDogs;
        this.foodAmountPerPounds = foodAmountPerPounds;
        this.foodPricePerPounds = foodPricePerPounds;
    }

    public String totalPrice() {
        return String.format(MONEY_SIGN + PRICE_FORMAT, getTotalPrice());
    }

    private double getTotalPrice() {
        return numberOfDogs * foodAmountPerPounds * foodPricePerPounds;
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
    @MethodSource("provideDogsAndFoodAmountAndFoodPrice")
    @DisplayName("총 사료 가격을 반환한다.")
    void feedCalculatesTheTotalFeedPrice(double numberOfDogs, double foodAmountPerPounds, double foodPricePerPounds, String expected) {
        Feed sut = new Feed(numberOfDogs, foodAmountPerPounds, foodPricePerPounds);

        String actual = sut.totalPrice();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideDogsAndFoodAmountAndFoodPrice() {
        return Stream.of(
                Arguments.of(3, 2, 1, "$6.00"),
                Arguments.of(3, 2, 3, "$18.00"),
                Arguments.of(5, .16, 4.54, "$3.63"),
                Arguments.of(3, 3.7, 3.6, "$39.96"),
                Arguments.of(1.5, 3.7, 3.6, "$19.98")
        );
    }
}

class Feed {

    private static final String MONEY_SIGN = "$";
    private static final String PRICE_FORMAT = "%.2f";

    private final double numberOfDogs;
    private final double foodAmountPerPounds;
    private final double foodPricePerPounds;

    public Feed(double numberOfDogs, double foodAmountPerPounds, double foodPricePerPounds) {
        this.numberOfDogs = numberOfDogs;
        this.foodAmountPerPounds = foodAmountPerPounds;
        this.foodPricePerPounds = foodPricePerPounds;
    }

    public String totalPrice() {
        return String.format(MONEY_SIGN + PRICE_FORMAT, getTotalPrice());
    }

    private double getTotalPrice() {
        return numberOfDogs * foodAmountPerPounds * foodPricePerPounds;
    }
}
~~~
