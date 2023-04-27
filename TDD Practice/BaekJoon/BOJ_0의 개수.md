# 0의 개수 (Bronze 1)

### 문제 설명

N부터 M까지의 수들을 종이에 적었을 때 종이에 적힌 0들을 세는 프로그램을 작성하라.

예를 들어, N, M이 각각 0, 10일 때 0을 세면 0에 하나, 10에 하나가 있으므로 답은 2이다.

출처 : https://www.acmicpc.net/problem/11170

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = scanner.nextInt();
        for (int i = 0; i < t; i++) {
            System.out.println(getCountOfZeroInRange(scanner.nextInt(), scanner.nextInt()));
        }
    }

    static int getCountOfZeroInRange(int start, int end) {
        return IntStream.rangeClosed(start, end)
                .map(Main::getCountOfZero)
                .sum();
    }

    static int getCountOfZero(int number) {
        if (number == 0) {
            return 1;
        }
        return getCountOfZeroWithoutZero(number);
    }

    static int getCountOfZeroWithoutZero(int number) {
        int result = 0;
        while (number > 0) {
            if (number % 10 == 0) {
                result++;
            }
            number /= 10;
        }
        return result;
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

import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideStartAndEnd")
    @DisplayName("주어진 숫자 범위에서 모든 0의 개수를 출력해야 한다.")
    void getCountOfZeroInRangeTest(int start, int end, int expected) {
        int actual = getCountOfZeroInRange(start, end);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideStartAndEnd() {
        return Stream.of(
                Arguments.of(0, 1, 1)
                , Arguments.of(0, 10, 2)
                , Arguments.of(33, 1005, 199)
                , Arguments.of(1, 4, 0)
                , Arguments.of(0, 1000000, 488896)
        );
    }

    @ParameterizedTest
    @MethodSource("provideNumber")
    @DisplayName("주어진 숫자의 0의 개수를 출력해야 한다.")
    void getCountOfZeroTest(int number, int expected) {
        int actual = getCountOfZero(number);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumber() {
        return Stream.of(
                Arguments.of(0, 1)
                , Arguments.of(100, 2)
                , Arguments.of(1001, 2)
                , Arguments.of(10101, 2)
                , Arguments.of(1000000, 6)
                , Arguments.of(999, 0)
        );
    }

    private int getCountOfZeroInRange(int start, int end) {
        return IntStream.rangeClosed(start, end)
                .map(this::getCountOfZero)
                .sum();
    }

    private int getCountOfZero(int number) {
        if (number == 0) {
            return 1;
        }
        return getCountOfZeroWithoutZero(number);
    }

    private int getCountOfZeroWithoutZero(int number) {
        int result = 0;
        while (number > 0) {
            if (number % 10 == 0) {
                result++;
            }
            number /= 10;
        }
        return result;
    }
}
~~~
