# 2의 제곱인가 (Bronze 3)

### 문제 설명

자연수 N이 주어졌을 때, 2의 제곱수면 1을 아니면 0을 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/11966

---

#### 풀이
~~~java
import java.math.BigInteger;
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        PowerOfTwo powerOfTwo = new PowerOfTwo();
        System.out.println(powerOfTwo.isValid(n));
    }
}

class PowerOfTwo {

    public static final int VALID = 1;
    public static final int INVALID = 0;

    public int isValid(int number) {
        return checkPowerOfTwo(number) ? VALID : INVALID;
    }

    private boolean checkPowerOfTwo(int number) {
        return new BigInteger(String.valueOf(number)).bitCount() == 1;
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

import java.math.BigInteger;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("주어진 숫자가 2의 제곱수라면 1을 아니라면 0을 반환한다.")
    void testIsPowerOfTwo(int number, int expected) {
        PowerOfTwo sut = new PowerOfTwo();

        int actual = sut.isValid(number);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(1, 1),
                Arguments.of(3, 0),
                Arguments.of(2, 1),
                Arguments.of(4, 1),
                Arguments.of(6, 0),
                Arguments.of(100, 0),
                Arguments.of(128, 1),
                Arguments.of(1073741824, 1)
        );
    }
}

class PowerOfTwo {

    public static final int VALID = 1;
    public static final int INVALID = 0;

    public int isValid(int number) {
        return checkPowerOfTwo(number) ? VALID : INVALID;
    }

    private boolean checkPowerOfTwo(int number) {
        return new BigInteger(String.valueOf(number)).bitCount() == 1;
    }
}
~~~
