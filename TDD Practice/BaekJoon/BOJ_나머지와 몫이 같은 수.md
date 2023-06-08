# 나머지와 몫이 같은 수 (Bronze 1)

### 문제 설명

N으로 나누었을 때 나머지와 몫이 같은 모든 자연수의 합을 구하는 프로그램을 작성하시오. 예를 들어 N=3일 때, 나머지와 몫이 모두 같은 자연수는 4와 8 두 개가 있으므로, 그 합은 12이다.

출처 : https://www.acmicpc.net/problem/1834

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(QuotientAndReminderEqualNumberCalculator.getSum(scanner.nextInt()));
    }
}

class QuotientAndReminderEqualNumberCalculator {

    public static long getSum(int n) {
        return LongStream.range(0, n)
                .map(i -> (n * i) + i)
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

import java.util.stream.LongStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideCandidates")
    @DisplayName("n으로 나누었을 때 나머지와 몫이 같은 모든 자연수의 합을 반환한다.")
    void getSumOfNumbersWithEqualQuotientAndReminderTest(int n, long expected) {
        long actual = QuotientAndReminderEqualNumberCalculator.getSum(n);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCandidates() {
        return Stream.of(
                Arguments.of(1, 0L),
                Arguments.of(3, 12L),
                Arguments.of(10, 495L),
                Arguments.of(2000000, 3999999999999000000L)
        );
    }
}

class QuotientAndReminderEqualNumberCalculator {

    public static long getSum(int n) {
        return LongStream.range(0, n)
                .map(i -> (n * i) + i)
                .sum();
    }
}
~~~
