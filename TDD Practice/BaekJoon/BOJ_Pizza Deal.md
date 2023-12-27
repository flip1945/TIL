# Pizza Deal (Bronze 4)

### 문제 설명

There’s a pizza store which serves pizza in two sizes: either a pizza slice, with area A1 and price P1, or a circular pizza, with radius R1 and price P2.

You want to maximize the amount of pizza you get per dollar. Should you pick the pizza slice or the whole pizza?

출처 : https://www.acmicpc.net/problem/16693

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        SlicePizza slicePizza = new SlicePizza(scanner.nextInt(), scanner.nextInt());
        WholePizza wholePizza = new WholePizza(scanner.nextInt(), scanner.nextInt());
        PizzaDeal pizzaDeal = new PizzaDeal(slicePizza, wholePizza);
        System.out.println(pizzaDeal.bestPizzaPerDollar());
    }
}

class PizzaDeal {
    private static final String SLICE_OF_PIZZA = "Slice of pizza";
    private static final String WHOLE_PIZZA = "Whole pizza";

    private final Pizza slicePizza;
    private final Pizza wholePizza;

    public PizzaDeal(Pizza slicePizza, Pizza wholePizza) {
        this.slicePizza = slicePizza;
        this.wholePizza = wholePizza;
    }

    public String bestPizzaPerDollar() {
        return isSlicePizzaBetterThenSlicePizza() ? SLICE_OF_PIZZA : WHOLE_PIZZA;
    }

    private boolean isSlicePizzaBetterThenSlicePizza() {
        return slicePizza.amountOfPizzaPerDollar() > wholePizza.amountOfPizzaPerDollar();
    }
}

interface Pizza {
    double amountOfPizzaPerDollar();
}

class SlicePizza implements Pizza {
    private final int area;
    private final int price;

    public SlicePizza(int area, int price) {
        this.area = area;
        this.price = price;
    }

    @Override
    public double amountOfPizzaPerDollar() {
        return (double) area / price;
    }
}

class WholePizza implements Pizza {
    private final int radius;
    private final int price;

    public WholePizza(int radius, int price) {
        this.radius = radius;
        this.price = price;
    }

    @Override
    public double amountOfPizzaPerDollar() {
        return getArea() / price;
    }

    private double getArea() {
        return radius * radius * Math.PI;
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
    @MethodSource("providePizzaAreaAndPrice")
    @DisplayName("피자의 크기와 가격이 주어지면 달러당 크기를 반환한다.")
    void slicePizzaReturnsAmountOfPizzaPerDollar(int area, int price, double expected) {
        Pizza sut = new SlicePizza(area, price);

        double actual = sut.amountOfPizzaPerDollar();

        assertEquals(expected, actual, 0.000001);
    }

    static Stream<Arguments> providePizzaAreaAndPrice() {
        return Stream.of(
                Arguments.of(4, 4, 1),
                Arguments.of(10, 3, 3.3333333333),
                Arguments.of(9, 2, 4.5)
        );
    }

    @ParameterizedTest
    @MethodSource("providePizzaRadiusAndPrice")
    @DisplayName("피자의 반지름과 가격이 주어지면 달러당 크기를 반환한다.")
    void wholePizzaReturnsAmountOfPizzaPerDollar(int radius, int price, double expected) {
        Pizza sut = new WholePizza(radius, price);

        double actual = sut.amountOfPizzaPerDollar();

        assertEquals(expected, actual, 0.000001);
    }

    static Stream<Arguments> providePizzaRadiusAndPrice() {
        return Stream.of(
                Arguments.of(2, 4, 3.141592),
                Arguments.of(7, 9, 17.1042266)
        );
    }

    @ParameterizedTest
    @MethodSource("provideSlicePizzaAndWholePizza")
    @DisplayName("어떤 피자가 더 달러당 많은 피자인지 반환한다.")
    void pizzaDealReturnTheBestPizzaPerDollar(SlicePizza slicePizza, WholePizza wholePizza, String expected) {
        PizzaDeal sut = new PizzaDeal(slicePizza, wholePizza);

        String actual = sut.bestPizzaPerDollar();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSlicePizzaAndWholePizza() {
        return Stream.of(
                Arguments.of(new SlicePizza(8, 4), new WholePizza(7, 9), "Whole pizza"),
                Arguments.of(new SlicePizza(841, 108), new WholePizza(8, 606), "Slice of pizza"),
                Arguments.of(new SlicePizza(9, 2), new WholePizza(4, 7), "Whole pizza")
        );
    }
}

class PizzaDeal {
    private static final String SLICE_OF_PIZZA = "Slice of pizza";
    private static final String WHOLE_PIZZA = "Whole pizza";

    private final Pizza slicePizza;
    private final Pizza wholePizza;

    public PizzaDeal(Pizza slicePizza, Pizza wholePizza) {
        this.slicePizza = slicePizza;
        this.wholePizza = wholePizza;
    }

    public String bestPizzaPerDollar() {
        return isSlicePizzaBetterThenSlicePizza() ? SLICE_OF_PIZZA : WHOLE_PIZZA;
    }

    private boolean isSlicePizzaBetterThenSlicePizza() {
        return slicePizza.amountOfPizzaPerDollar() > wholePizza.amountOfPizzaPerDollar();
    }
}

interface Pizza {
    double amountOfPizzaPerDollar();
}

class SlicePizza implements Pizza {
    private final int area;
    private final int price;

    public SlicePizza(int area, int price) {
        this.area = area;
        this.price = price;
    }

    @Override
    public double amountOfPizzaPerDollar() {
        return (double) area / price;
    }
}

class WholePizza implements Pizza {
    private final int radius;
    private final int price;

    public WholePizza(int radius, int price) {
        this.radius = radius;
        this.price = price;
    }

    @Override
    public double amountOfPizzaPerDollar() {
        return getArea() / price;
    }

    private double getArea() {
        return radius * radius * Math.PI;
    }
}
~~~
