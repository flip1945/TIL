# 백대열 (Silver 5)

### 문제 설명

대열이는 욱제의 친구다.

* “야 백대열을 약분하면 뭔지 알아?”
* “??”
* “십대일이야~ 하하!”

n:m이 주어진다. 욱제를 도와주자. (...)

출처 : https://www.acmicpc.net/problem/14490

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(new ReducingFraction(scanner.nextLine()));
    }
}

class ReducingFraction {

    public static final String FRACTION_DELIMITER = ":";
    public static final int FIRST_NUMBER_INDEX = 0;
    public static final int SECOND_NUMBER_INDEX = 1;
    private final int firstNumber;
    private final int secondNumber;
    private final Gcd gcd;

    public ReducingFraction(String fraction) {
        String[] split = fraction.split(FRACTION_DELIMITER);
        this.firstNumber = Integer.parseInt(split[FIRST_NUMBER_INDEX]);
        this.secondNumber = Integer.parseInt(split[SECOND_NUMBER_INDEX]);
        this.gcd = Gcd.of(firstNumber, secondNumber);
    }

    @Override
    public String toString() {
        int gcdNumber = gcd.getGcd();
        return firstNumber / gcdNumber + FRACTION_DELIMITER + secondNumber / gcdNumber;
    }
}

class Gcd {

    private final int small;
    private final int big;

    private Gcd(int small, int big) {
        this.small = small;
        this.big = big;
    }

    public static Gcd of(int small, int big) {
        return new Gcd(small, big);
    }

    public int getGcd() {
        return next(small, big).small;
    }

    private Gcd next(int small, int big) {
        if (big % small == 0) {
            return new Gcd(small, big);
        }
        return next(big % small, small);
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
    @MethodSource("provideFractions")
    @DisplayName("두 수를 최대한 약분한 숫자를 반환한다.")
    void reducingFractionTest(String fraction, String expected) {
        ReducingFraction sut = new ReducingFraction(fraction);
        
        String actual = sut.toString();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideFractions() {
        return Stream.of(
                Arguments.of("100:10", "10:1"),
                Arguments.of("6:4", "3:2"),
                Arguments.of("18:24", "3:4"),
                Arguments.of("1:1", "1:1")
        );
    }

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("두 수의 최대공약수를 반환한다.")
    void gcdTest(int number1, int number2, int expected) {
        Gcd sut = Gcd.of(number1, number2);

        int actual = sut.getGcd();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(6, 4, 2),
                Arguments.of(16, 12, 4),
                Arguments.of(18, 24, 6),
                Arguments.of(100, 10, 10),
                Arguments.of(10, 10, 10)
        );
    }
}

class ReducingFraction {

    public static final String FRACTION_DELIMITER = ":";
    public static final int FIRST_NUMBER_INDEX = 0;
    public static final int SECOND_NUMBER_INDEX = 1;
    private final int firstNumber;
    private final int secondNumber;
    private final Gcd gcd;

    public ReducingFraction(String fraction) {
        String[] split = fraction.split(FRACTION_DELIMITER);
        this.firstNumber = Integer.parseInt(split[FIRST_NUMBER_INDEX]);
        this.secondNumber = Integer.parseInt(split[SECOND_NUMBER_INDEX]);
        this.gcd = Gcd.of(firstNumber, secondNumber);
    }

    @Override
    public String toString() {
        int gcdNumber = gcd.getGcd();
        return firstNumber / gcdNumber + FRACTION_DELIMITER + secondNumber / gcdNumber;
    }
}

class Gcd {

    private final int small;
    private final int big;

    private Gcd(int small, int big) {
        this.small = small;
        this.big = big;
    }

    public static Gcd of(int small, int big) {
        return new Gcd(small, big);
    }

    public int getGcd() {
        return next(small, big).small;
    }

    private Gcd next(int small, int big) {
        if (big % small == 0) {
            return new Gcd(small, big);
        }
        return next(big % small, small);
    }
}
~~~
