# 배수들의 합 (Bronze 2)

### 문제 설명

신원이는 백준에서 배수에 관한 문제를 풀다가 감명을 받아 새로운 문제를 만들어보았다. 자연수 N과 M개의 자연수 Ki가 주어진다. Ki중 적어도 하나의 배수이면서 1 이상 N 이하인 수의 합을 구하여라.

출처 : https://www.acmicpc.net/problem/17173

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int m = scanner.nextInt();
        List<Integer> k = IntStream.range(0, m)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());

        System.out.println(getSumOfMultiple(n, k));
    }

    static int getSumOfMultiple(int n, List<Integer> numbers) {
        return IntStream.rangeClosed(1, n)
                .filter(num -> isMultipleOfNumber(numbers, num))
                .sum();
    }

    static boolean isMultipleOfNumber(List<Integer> numbers, int number) {
        return numbers.stream()
                .anyMatch(num -> number % num == 0);
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

import java.util.List;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("1부터 n까지의 숫자 중에서 주어진 숫자들 중 적어도 하나의 배수에 해당하는 숫자의 합을 반환한다.")
    void getSumOfMultipleTest(int n, List<Integer> numbers, int expected) {
        int actual = getSumOfMultiple(n, numbers);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(6, List.of(2), 12)
                , Arguments.of(6, List.of(3), 9)
                , Arguments.of(6, List.of(2, 3), 15)
                , Arguments.of(10, List.of(2, 3), 42)
                , Arguments.of(1000, List.of(3, 5, 7), 272066)
        );
    }

    private int getSumOfMultiple(int n, List<Integer> numbers) {
        return IntStream.rangeClosed(1, n)
                .filter(num -> isMultipleOfNumber(numbers, num))
                .sum();
    }

    private boolean isMultipleOfNumber(List<Integer> numbers, int number) {
        return numbers.stream()
                .anyMatch(num -> number % num == 0);
    }
}
~~~
