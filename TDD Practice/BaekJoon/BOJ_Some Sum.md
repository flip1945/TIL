# Some Sum (Bronze 3)

### 문제 설명

Your friend has secretly picked 
$N$ consecutive positive integers between 
$1$ and 
$100$, and wants you to guess if their sum is even or odd.

If the sum must be even, output 'Even'.  If the sum must be odd, output 'Odd'.  If the sum could be even or could be odd, output 'Either'.

출처 : https://www.acmicpc.net/problem/21185

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        SomeSum someSum = new SomeSum();
        System.out.println(someSum.check(n));
    }
}

class SomeSum {

    private static final String ODD = "Odd";
    private static final String EVEN = "Even";
    private static final String EITHER = "Either";

    public String check(int number) {
        if (number % 2 == 0) {
            return getOddOrEven(number);
        }
        return EITHER;
    }

    private String getOddOrEven(int number) {
        if (isEvenSum(number)) {
            return EVEN;
        }
        return ODD;
    }

    private boolean isEvenSum(int number) {
        return number % 4 == 0;
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
    @MethodSource("provideNumber")
    @DisplayName("연속된 숫자의 길이가 주어졌을 때 숫자들의 합이 홀수인지 짝수인지 아니면 둘 다 가능한지 결과를 반환한다.")
    void shouldReturnWhetherTheSumOfTheNumbersIsOddOrEvenOrEither(int number, String expected) {
        SomeSum sut = new SomeSum();

        String actual = sut.check(number);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumber() {
        return Stream.of(
                Arguments.of(1, "Either"),
                Arguments.of(2, "Odd"),
                Arguments.of(3, "Either"),
                Arguments.of(4, "Even"),
                Arguments.of(5, "Either"),
                Arguments.of(6, "Odd")
        );
    }
}

class SomeSum {

    private static final String ODD = "Odd";
    private static final String EVEN = "Even";
    private static final String EITHER = "Either";

    public String check(int number) {
        if (number % 2 == 0) {
            return getOddOrEven(number);
        }
        return EITHER;
    }

    private String getOddOrEven(int number) {
        if (isEvenSum(number)) {
            return EVEN;
        }
        return ODD;
    }

    private boolean isEvenSum(int number) {
        return number % 4 == 0;
    }
}
~~~
