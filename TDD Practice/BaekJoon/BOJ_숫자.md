# 숫자 (Bronze 2)

### 문제 설명

두 양의 정수가 주어졌을 때, 두 수 사이에 있는 정수를 모두 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/10093

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        BetweenNumber betweenNumber = BetweenNumber.of(scanner.nextLong(), scanner.nextLong());

        System.out.println(betweenNumber.count());
        System.out.println(betweenNumber);
    }
}

class BetweenNumber {

    private static final long FROM_OFFSET = 1L;
    private static final String STRING_DELIMITER = " ";

    private final List<Long> numbers;

    private BetweenNumber(List<Long> numbers) {
        this.numbers = numbers;
    }

    public static BetweenNumber of(long number1, long number2) {
        long from = Math.min(number1, number2);
        long to = Math.max(number1, number2);
        return new BetweenNumber(initNumbers(from, to));
    }

    private static List<Long> initNumbers(long from, long to) {
        return LongStream.range(from + FROM_OFFSET, to)
                .boxed()
                .collect(Collectors.toList());
    }

    public long count() {
        return numbers.size();
    }

    public List<Long> list() {
        return numbers;
    }

    @Override
    public String toString() {
        return numbers.stream()
                .map(String::valueOf)
                .collect(Collectors.joining(STRING_DELIMITER));
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.LongStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideBetweenNumbersAndExpectedCount")
    @DisplayName("두 수 사이의 개수를 반환한다.")
    void testBetweenNumbersCount(long number1, long number2, long expected) {
        BetweenNumber sut = BetweenNumber.of(number1, number2);

        long actual = sut.count();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideBetweenNumbersAndExpectedCount() {
        return Stream.of(
                Arguments.of(1, 3, 1),
                Arguments.of(1, 4, 2),
                Arguments.of(4, 1, 2),
                Arguments.of(1, 1, 0),
                Arguments.of(8, 14, 5),
                Arguments.of(1, 100001, 99999),
                Arguments.of(100, 1000, 899),
                Arguments.of(100000000000000L, 100000000100000L, 99999)
        );
    }

    @ParameterizedTest
    @MethodSource("provideBetweenNumbersAndExpectedList")
    @DisplayName("두 수 사이의 목록을 반환한다.")
    void testBetweenNumbersList(long number1, long number2, List<Long> expected) {
        BetweenNumber sut = BetweenNumber.of(number1, number2);

        List<Long> actual = sut.list();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideBetweenNumbersAndExpectedList() {
        return Stream.of(
                Arguments.of(1, 3, List.of(2L)),
                Arguments.of(1, 4, List.of(2L, 3L)),
                Arguments.of(8, 14, List.of(9L, 10L, 11L, 12L, 13L)),
                Arguments.of(14, 8, List.of(9L, 10L, 11L, 12L, 13L)),
                Arguments.of(100, 100, List.of())
        );
    }

    @Test
    @DisplayName("두 수 사이의 목록을 문자열로 반환한다.")
    void testToString() {
        BetweenNumber sut = BetweenNumber.of(1, 11);

        String actual = sut.toString();

        assertEquals("2 3 4 5 6 7 8 9 10", actual);
    }
}

class BetweenNumber {

    private static final long FROM_OFFSET = 1L;
    private static final String STRING_DELIMITER = " ";

    private final List<Long> numbers;

    private BetweenNumber(List<Long> numbers) {
        this.numbers = numbers;
    }

    public static BetweenNumber of(long number1, long number2) {
        long from = Math.min(number1, number2);
        long to = Math.max(number1, number2);
        return new BetweenNumber(initNumbers(from, to));
    }

    private static List<Long> initNumbers(long from, long to) {
        return LongStream.range(from + FROM_OFFSET, to)
                .boxed()
                .collect(Collectors.toList());
    }

    public long count() {
        return numbers.size();
    }

    public List<Long> list() {
        return numbers;
    }

    @Override
    public String toString() {
        return numbers.stream()
                .map(String::valueOf)
                .collect(Collectors.joining(STRING_DELIMITER));
    }
}
~~~
