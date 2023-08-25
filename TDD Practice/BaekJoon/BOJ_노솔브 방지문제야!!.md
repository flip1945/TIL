# 노솔브 방지문제야!! (Bronze 3)

### 문제 설명

여러분은 Q개의 쿼리를 수행해야 합니다. 수행해야 하는 쿼리는 다음과 같습니다.

어떤 수 a를 2의 거듭제곱 꼴로 나타낼 수 있는가?

출처 : https://www.acmicpc.net/problem/15917

---

#### 풀이
~~~java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(bufferedReader.readLine());
        StringBuilder answer = new StringBuilder();

        for (int i = 0; i < n; i++) {
            int number = Integer.parseInt(bufferedReader.readLine());
            PowerOfTwo powerOfTwo = new PowerOfTwo(number);
            answer.append(powerOfTwo.isValid()).append("\n");
        }
        System.out.println(answer);
    }
}

class PowerOfTwo {

    private static final int TRUE = 1;
    private static final int FALSE = 0;

    private final int number;

    public PowerOfTwo(int number) {
        this.number = number;
    }

    public int isValid() {
        return isPowerOfTwo() ? TRUE : FALSE;
    }

    private boolean isPowerOfTwo() {
        return (number & (number - 1)) == 0;
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
    @DisplayName("주어진 숫자가 2의 거듭제곱이라면 1 아니라면 0을 반환한다.")
    void shouldReturnOneIfTheGivenNumberIsAPowerOfTwoElseZero(int number, int expected) {
        PowerOfTwo sut = new PowerOfTwo(number);

        int actual = sut.isValid();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumber() {
        return Stream.of(
                Arguments.of(1, 1),
                Arguments.of(2, 1),
                Arguments.of(3, 0),
                Arguments.of(4, 1),
                Arguments.of(5, 0),
                Arguments.of(6, 0),
                Arguments.of(7, 0),
                Arguments.of(8, 1),
                Arguments.of(14, 0),
                Arguments.of(32, 1),
                Arguments.of(33, 0),
                Arguments.of(34, 0),
                Arguments.of(35, 0),
                Arguments.of(36, 0),
                Arguments.of(64, 1),
                Arguments.of(128, 1),
                Arguments.of(256, 1),
                Arguments.of(512, 1),
                Arguments.of(1024, 1),
                Arguments.of(1073741824, 1),
                Arguments.of(2147483647, 0)
        );
    }
}

class PowerOfTwo {

    private static final int TRUE = 1;
    private static final int FALSE = 0;

    private final int number;

    public PowerOfTwo(int number) {
        this.number = number;
    }

    public int isValid() {
        return isPowerOfTwo() ? TRUE : FALSE;
    }

    private boolean isPowerOfTwo() {
        return (number & (number - 1)) == 0;
    }
}
~~~
