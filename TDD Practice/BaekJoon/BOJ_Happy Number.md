# Happy Number (Bronze 2)

### 문제 설명

Consider the following function f defined for any natural number n:

> f(n) is the number obtained by summing up the squares of the digits of n in decimal (or base-ten).

If n = 19, for example, then f(19) = 82 because 1^2 + 9^2 = 82.

Repeatedly applying this function f, some natural numbers eventually become 1. Such numbers are called happy numbers. For example, 19 is a happy number, because repeatedly applying function f to 19 results in:

* f(19) = 1^2 + 9^2 = 82
* f(82) = 8^2 + 2^2 = 68
* f(68) = 6^2 + 8^2 = 100
* f(100) = 1^2 + 0^2 + 0^2 = 1

However, not all natural numbers are happy. You could try 5 and you will see that 5 is not a happy number. If n is not a happy number, it has been proved by mathematicians that repeatedly applying function f to n reaches the following cycle:

> 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4.

Write a program that decides if a given natural number n is a happy number or not.

출처 : https://www.acmicpc.net/problem/14954

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(isHappyNumber(scanner.nextInt()));
    }

    static String isHappyNumber(int number) {
        number = getSumOfSquaresOfEachDigit(number);
        while (isNotUnhappy(number)) {
            if (isHappy(number)) {
                return "HAPPY";
            }
            number = getSumOfSquaresOfEachDigit(number);
        }
        return "UNHAPPY";
    }

    static int getSumOfSquaresOfEachDigit(int number) {
        int result = 0;
        while (number > 0) {
            result += Math.pow(number % 10, 2);
            number /= 10;
        }
        return result;
    }

    static boolean isNotUnhappy(int number) {
        return number != 4;
    }

    static boolean isHappy(int number) {
        return number == 1;
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
import org.junit.jupiter.params.provider.ValueSource;

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @ValueSource(ints = {19, 82, 100, 1000})
    @DisplayName("사이클이 없는 숫자는 HAPPY NUMBER 이다.")
    void isHappyNumberTest(int number) {
        String actual = isHappyNumber(number);

        assertEquals("HAPPY", actual);
    }

    @ParameterizedTest
    @ValueSource(ints = {4, 5, 200, 999999999})
    @DisplayName("사이클이 있는 숫자는 UNHAPPY NUMBER 이다.")
    void isUnhappyNumberTest(int number) {
        String actual = isHappyNumber(number);

        assertEquals("UNHAPPY", actual);
    }

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("각 자릿수의 제곱의 합을 반환한다.")
    void getSumOfSquaresOfEachDigitTest(int number, int expected) {
        int actual = getSumOfSquaresOfEachDigit(number);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(100, 1)
                , Arguments.of(19, 82)
                , Arguments.of(82, 68)
                , Arguments.of(4, 16)
        );
    }

    private String isHappyNumber(int number) {
        number = getSumOfSquaresOfEachDigit(number);
        while (isNotUnhappy(number)) {
            if (isHappy(number)) {
                return "HAPPY";
            }
            number = getSumOfSquaresOfEachDigit(number);
        }
        return "UNHAPPY";
    }

    private int getSumOfSquaresOfEachDigit(int number) {
        int result = 0;
        while (number > 0) {
            result += Math.pow(number % 10, 2);
            number /= 10;
        }
        return result;
    }

    private boolean isNotUnhappy(int number) {
        return number != 4;
    }

    private boolean isHappy(int number) {
        return number == 1;
    }
}
~~~
