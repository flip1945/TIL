# 반올림 (Bronze 1)

### 문제 설명

정수 N이 주어져 있을 때 이 수가 10보다 크면 일의 자리에서 반올림을 하고, 이 결과가 100보다 크면 다시 10의 자리에서 반올림을 하고, 또 이 수가 1000보다 크면 100의 자리에서 반올림을 하고.. (이하 생략) 이러한 연산을 한 결과를 출력하시오.

출처 : https://www.acmicpc.net/problem/2033

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        RoundingCalculator calculator = new RoundingCalculator();
        System.out.println(calculator.calculate(scanner.nextInt()));
    }
}

class RoundingCalculator {
    public int calculate(int number) {
        if (number < 10) {
            return number;
        }

        return calculateOverTen(number);
    }

    private int calculateOverTen(int number) {
        int result = 1;
        while (number >= 10) {
            int remainder = number % 10;
            if (remainder >= 5) {
                number += 10;
            }
            number /= 10;
            result *= 10;
        }
        return result * number;
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
    @MethodSource("provideNumbers")
    @DisplayName("모든 숫자를 반올림한 결과를 반환한다.")
    void returns_the_result_of_rounding_number(int number, int expected) {
        RoundingCalculator sut = new RoundingCalculator();

        int actual = sut.calculate(number);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(15, 20),
                Arguments.of(14, 10),
                Arguments.of(60, 60),
                Arguments.of(5, 5),
                Arguments.of(466, 500),
                Arguments.of(0, 0),
                Arguments.of(99999999, 100000000)
        );
    }
}

class RoundingCalculator {
    public int calculate(int number) {
        if (number < 10) {
            return number;
        }

        return calculateOverTen(number);
    }

    private int calculateOverTen(int number) {
        int result = 1;
        while (number >= 10) {
            int remainder = number % 10;
            if (remainder >= 5) {
                number += 10;
            }
            number /= 10;
            result *= 10;
        }
        return result * number;
    }
}
~~~
