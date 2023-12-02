# Digit Count (Bronze 3)

### 문제 설명

There are many ways to count the frequencies of letters but can they be applied to digits?

Given a range (in the form of two integers) and a digit (0-9), you are to count how many occurrences of the digit there are in the given range.

출처 : https://www.acmicpc.net/problem/25841

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int from = scanner.nextInt();
        int to = scanner.nextInt();
        int target = scanner.nextInt();

        Range range = new Range(from, to);
        NumberCounter numberCounter = new NumberCounter(target);
        RangeCounter rangeCounter = new RangeCounter(range, numberCounter);
        System.out.println(rangeCounter.count());
    }
}

class RangeCounter {

    private final Range range;
    private final NumberCounter counter;

    public RangeCounter(Range range, NumberCounter counter) {
        this.range = range;
        this.counter = counter;
    }

    public int count() {
        return range.getRangeList().stream()
                .mapToInt(counter::count)
                .sum();
    }
}

class Range {

    private final int from;
    private final int to;

    public Range(int from, int to) {
        this.from = from;
        this.to = to;
    }

    public List<Integer> getRangeList() {
        return IntStream.rangeClosed(from, to)
                .boxed()
                .collect(Collectors.toList());
    }
}

class NumberCounter {

    private final int target;

    public NumberCounter(int target) {
        this.target = target;
    }

    public int count(Integer from) {
        return (int) String.valueOf(from).chars()
                .filter(this::isTargetNumber)
                .count();
    }

    private boolean isTargetNumber(int ch) {
        return Character.getNumericValue(ch) == target;
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

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumber")
    @DisplayName("숫자가 주어지면 특정 숫자의 개수를 반한한다.")
    void numberCounterCountsTheNumber(int number, int target, int expected) {
        NumberCounter sut = new NumberCounter(target);

        int actual = sut.count(number);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideNumber() {
        return Stream.of(
                Arguments.of(1000, 0, 3),
                Arguments.of(100, 0, 2),
                Arguments.of(100, 2, 0)
        );
    }

    @ParameterizedTest
    @MethodSource("provideRange")
    @DisplayName("숫자 범위가 주어지면 특정 숫자의 개수를 반한한다.")
    void rangeCounterCountsTheRangedNumbers(Range range, int target, int expected) {
        NumberCounter counter = new NumberCounter(target);
        RangeCounter sut = new RangeCounter(range, counter);

        int actual = sut.count();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideRange() {
        return Stream.of(
                Arguments.of(new Range(1000, 1000), 0, 3),
                Arguments.of(new Range(1000, 1001), 0, 5),
                Arguments.of(new Range(8996, 9004), 5, 0),
                Arguments.of(new Range(9800, 9900), 5, 20),
                Arguments.of(new Range(9999, 1), 5, 0)
        );
    }
}

class RangeCounter {

    private final Range range;
    private final NumberCounter counter;

    public RangeCounter(Range range, NumberCounter counter) {
        this.range = range;
        this.counter = counter;
    }

    public int count() {
        return range.getRangeList().stream()
                .mapToInt(counter::count)
                .sum();
    }
}

class Range {

    private final int from;
    private final int to;

    public Range(int from, int to) {
        this.from = from;
        this.to = to;
    }

    public List<Integer> getRangeList() {
        return IntStream.rangeClosed(from, to)
                .boxed()
                .collect(Collectors.toList());
    }
}

class NumberCounter {

    private final int target;

    public NumberCounter(int target) {
        this.target = target;
    }

    public int count(Integer from) {
        return (int) String.valueOf(from).chars()
                .filter(this::isTargetNumber)
                .count();
    }

    private boolean isTargetNumber(int ch) {
        return Character.getNumericValue(ch) == target;
    }
}
~~~
