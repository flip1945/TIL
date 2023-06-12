# 최소공배수 (Silver 5)

### 문제 설명

정수 B에 0보다 큰 정수인 N을 곱해 정수 A를 만들 수 있다면, A는 B의 배수이다.

예:
* 10은 5의 배수이다 (5*2 = 10)
* 10은 10의 배수이다(10*1 = 10)
* 6은 1의 배수이다(1*6 = 6)
* 20은 1, 2, 4,5,10,20의 배수이다.

다른 예:
* 2와 5의 최소공배수는 10이고, 그 이유는 2와 5보다 작은 공배수가 없기 때문이다.
* 10과 20의 최소공배수는 20이다.
* 5와 3의 최소공배수는 15이다.

당신은 두 수에 대하여 최소공배수를 구하는 프로그램을 작성 하는 것이 목표이다.

출처 : https://www.acmicpc.net/problem/13241

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        LcmCalculator lcmCalculator = new LcmCalculator(new GcdCalculator());
        System.out.println(lcmCalculator.calculate(scanner.nextInt(), scanner.nextInt()));
    }
}

class LcmCalculator {

    private final GcdCalculator gcdCalculator;

    public LcmCalculator(GcdCalculator gcdCalculator) {
        this.gcdCalculator = gcdCalculator;
    }

    public long calculate(long number1, long number2) {
        return (number1 * number2) / gcdCalculator.calculate(number1, number2);
    }
}

class GcdCalculator {

    public long calculate(long small, long big) {
        if (big % small == 0) {
            return small;
        }
        return calculate(big % small, small);
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
    @DisplayName("두 수의 최소공배수를 반환한다.")
    void lcmCalculateTest(long number1, long number2, long expected) {
        LcmCalculator sut = new LcmCalculator(new GcdCalculator());

        long actual = sut.calculate(number1, number2);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(1, 1, 1),
                Arguments.of(3, 5, 15),
                Arguments.of(1, 123, 123),
                Arguments.of(4, 6, 12),
                Arguments.of(121, 199, 24079)
        );
    }
}

class LcmCalculator {

    private final GcdCalculator gcdCalculator;

    public LcmCalculator(GcdCalculator gcdCalculator) {
        this.gcdCalculator = gcdCalculator;
    }

    public long calculate(long number1, long number2) {
        return (number1 * number2) / gcdCalculator.calculate(number1, number2);
    }
}

class GcdCalculator {

    public long calculate(long small, long big) {
        if (big % small == 0) {
            return small;
        }
        return calculate(big % small, small);
    }
}
~~~
