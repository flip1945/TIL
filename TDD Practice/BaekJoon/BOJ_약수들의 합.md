# 약수들의 합 (Bronze 1)

### 문제 설명

어떤 숫자 n이 자신을 제외한 모든 약수들의 합과 같으면, 그 수를 완전수라고 한다.

예를 들어 6은 6 = 1 + 2 + 3 으로 완전수이다.

n이 완전수인지 아닌지 판단해주는 프로그램을 작성하라.

출처 : https://www.acmicpc.net/problem/9506

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        while (n != -1) {
            System.out.println(checkPerfectNumber(n));
            n = scanner.nextInt();
        }
    }

    static String checkPerfectNumber(int number) {
        List<Integer> divisors = getDivisors(number);
        if (isNotPerfectNumber(number, divisors)) {
            return number + " is NOT perfect.";
        }

        return getPerfectNumberExpression(number, divisors);
    }

    static List<Integer> getDivisors(int number) {
        return IntStream.range(1, number)
                .filter(i -> number % i == 0)
                .boxed()
                .collect(Collectors.toList());
    }

    static boolean isNotPerfectNumber(int number, List<Integer> divisors) {
        int sumOfDivisors = divisors.stream()
                .mapToInt(Integer::intValue)
                .sum();
        return sumOfDivisors != number;
    }

    static String getPerfectNumberExpression(int number, List<Integer> divisors) {
        return number + " = " + divisors.stream()
                .map(String::valueOf)
                .reduce((a, b) -> a + " + " + b)
                .orElse("");
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
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("숫자의 약수들을 반환해야 한다.")
    void getDivisorsTest(int number, List<Integer> expected) {
        List<Integer> actual = getDivisors(number);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(6, List.of(1, 2, 3)),
                Arguments.of(12, List.of(1, 2, 3, 4, 6)),
                Arguments.of(28, List.of(1, 2, 4, 7, 14)),
                Arguments.of(5, List.of(1))
        );
    }

    @ParameterizedTest
    @MethodSource("providePerfectNumbers")
    @DisplayName("완전수인 경우 숫자의 합계를 수식으로 반환해야 한다.")
    void checkPerfectNumberTest(int number, String expected) {
        String actual = checkPerfectNumber(number);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> providePerfectNumbers() {
        return Stream.of(
                Arguments.of(6, "6 = 1 + 2 + 3"),
                Arguments.of(28, "28 = 1 + 2 + 4 + 7 + 14"),
                Arguments.of(496, "496 = 1 + 2 + 4 + 8 + 16 + 31 + 62 + 124 + 248"),
                Arguments.of(8128, "8128 = 1 + 2 + 4 + 8 + 16 + 32 + 64 + 127 + 254 + 508 + 1016 + 2032 + 4064")
        );
    }

    @ParameterizedTest
    @MethodSource("provideNotPerfectNumbers")
    @DisplayName("완전수가 아닌 경우 완벽수가 아니라고 반환해야 한다.")
    void checkNotPerfectNumberTest(int number, String expected) {
        String actual = checkPerfectNumber(number);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNotPerfectNumbers() {
        return Stream.of(
                Arguments.of(12, "12 is NOT perfect."),
                Arguments.of(1, "1 is NOT perfect."),
                Arguments.of(10, "10 is NOT perfect."),
                Arguments.of(2, "2 is NOT perfect.")
        );
    }

    private String checkPerfectNumber(int number) {
        List<Integer> divisors = getDivisors(number);
        if (isNotPerfectNumber(number, divisors)) {
            return number + " is NOT perfect.";
        }

        return getPerfectNumberExpression(number, divisors);
    }

    private List<Integer> getDivisors(int number) {
        return IntStream.range(1, number)
                .filter(i -> number % i == 0)
                .boxed()
                .collect(Collectors.toList());
    }

    private boolean isNotPerfectNumber(int number, List<Integer> divisors) {
        int sumOfDivisors = divisors.stream()
                .mapToInt(Integer::intValue)
                .sum();
        return sumOfDivisors != number;
    }

    private String getPerfectNumberExpression(int number, List<Integer> divisors) {
        return number + " = " + divisors.stream()
                .map(String::valueOf)
                .reduce((a, b) -> a + " + " + b)
                .orElse("");
    }
}
~~~
