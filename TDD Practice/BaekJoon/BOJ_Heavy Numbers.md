# Heavy Numbers (Bronze 3)

### 문제 설명

Consider a positive integer a. We define weight of a as:

(number of digits in a) * (sum of the digits in a)

For example, if a = 5767, then weight of a is:

(4) * (5 + 7 + 6 + 7) = 100

Given two positive integers, determine which one weighs more, i.e., it is heavier.

출처 : https://www.acmicpc.net/problem/25814

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int firstNumber = scanner.nextInt();
        int secondNumber = scanner.nextInt();

        HeavyNumbers heavyNumbers = HeavyNumbers.of(firstNumber, secondNumber);
        System.out.println(heavyNumbers.compareTwoNumbers());
    }
}

class HeavyNumbers {

    private static final int SAME = 0;
    private static final int FIRST = 1;
    private static final int SECOND = 2;

    private final NumberWeight firstNumberWeight;
    private final NumberWeight secondNumberWeight;

    public HeavyNumbers(NumberWeight firstNumberWeight, NumberWeight secondNumberWeight) {
        this.firstNumberWeight = firstNumberWeight;
        this.secondNumberWeight = secondNumberWeight;
    }

    public static HeavyNumbers of(int firstNumber, int secondNumber) {
        return new HeavyNumbers(new NumberWeight(firstNumber), new NumberWeight(secondNumber));
    }

    public int compareTwoNumbers() {
        if (firstNumberWeight.weight() == secondNumberWeight.weight()) {
            return SAME;
        }
        return (firstNumberWeight.weight() > secondNumberWeight.weight()) ? FIRST : SECOND;
    }
}

class NumberWeight {

    private final String number;

    public NumberWeight(int number) {
        this.number = String.valueOf(number);
    }

    public int weight() {
        return getSumOfDigits() * number.length();
    }

    private int getSumOfDigits() {
        return String.valueOf(number)
                .chars()
                .map(Character::getNumericValue)
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

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideTwoNumbers")
    @DisplayName("두 단어의 무게를 비교한다.")
    void heavyNumbersCompareTwoNumbersWeight(int firstNumber, int secondNumber, int expected) {
        HeavyNumbers sut = HeavyNumbers.of(firstNumber, secondNumber);

        int actual = sut.compareTwoNumbers();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideTwoNumbers() {
        return Stream.of(
                Arguments.of(12, 34, 2),
                Arguments.of(8, 567, 2),
                Arguments.of(59, 1001, 1),
                Arguments.of(123, 90, 0)
        );
    }

    @ParameterizedTest
    @MethodSource("provideNumber")
    @DisplayName("주어진 단어의 무게를 반환한다.")
    void numberWeightCalculatesWeight(int number, int expected) {
        NumberWeight sut = new NumberWeight(number);

        int actual = sut.weight();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideNumber() {
        return Stream.of(
                Arguments.of(12, 6),
                Arguments.of(34, 14),
                Arguments.of(5767, 100),
                Arguments.of(59, 28),
                Arguments.of(1001, 8)
        );
    }
}

class HeavyNumbers {

    private static final int SAME = 0;
    private static final int FIRST = 1;
    private static final int SECOND = 2;

    private final NumberWeight firstNumberWeight;
    private final NumberWeight secondNumberWeight;

    public HeavyNumbers(NumberWeight firstNumberWeight, NumberWeight secondNumberWeight) {
        this.firstNumberWeight = firstNumberWeight;
        this.secondNumberWeight = secondNumberWeight;
    }

    public static HeavyNumbers of(int firstNumber, int secondNumber) {
        return new HeavyNumbers(new NumberWeight(firstNumber), new NumberWeight(secondNumber));
    }

    public int compareTwoNumbers() {
        if (firstNumberWeight.weight() == secondNumberWeight.weight()) {
            return SAME;
        }
        return (firstNumberWeight.weight() > secondNumberWeight.weight()) ? FIRST : SECOND;
    }
}

class NumberWeight {

    private final String number;

    public NumberWeight(int number) {
        this.number = String.valueOf(number);
    }

    public int weight() {
        return getSumOfDigits() * number.length();
    }

    private int getSumOfDigits() {
        return String.valueOf(number)
                .chars()
                .map(Character::getNumericValue)
                .sum();
    }
}
~~~
