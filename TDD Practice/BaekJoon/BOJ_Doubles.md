# Doubles (Bronze 1)

### 문제 설명

2~15개의 서로 다른 자연수로 이루어진 리스트가 있을 때, 이들 중 리스트 안에 자신의 정확히 2배인 수가 있는 수의 개수를 구하여라.

예를 들어, 리스트가 "1 4 3 2 9 7 18 22"라면 2가 1의 2배, 4가 2의 2배, 18이 9의 2배이므로 답은 3이다.

출처 : https://www.acmicpc.net/problem/4641

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String input = scanner.nextLine();

        while (!input.equals("-1")) {
            Doubles doubles = Doubles.from(inputNumbers(input));
            System.out.println(doubles.count());

            input = scanner.nextLine();
        }
    }

    private static List<Integer> inputNumbers(String input) {
        return Stream.of(input.split(" "))
                .map(Integer::parseInt)
                .collect(Collectors.toList());
    }
}

class Doubles {

    public static final int EXCLUDE_NUMBER = 0;
    public static final int HALF_LIMIT = 50;
    public static final int LIMIT = 101;
    private final List<Integer> numbers;
    private final boolean[] numberExists = new boolean[LIMIT];

    private Doubles(List<Integer> numbers) {
        this.numbers = numbers;
        initNumberExists(numbers);
    }

    public static Doubles from(List<Integer> numbers) {
        return new Doubles(numbers);
    }

    private void initNumberExists(List<Integer> numbers) {
        for (int number : numbers) {
            numberExists[number] = true;
        }
    }

    public int count() {
        return (int) numbers.stream()
                .filter(this::isDouble)
                .count();
    }

    private boolean isDouble(Integer number) {
        return number != EXCLUDE_NUMBER && number <= HALF_LIMIT && numberExists[number * 2];
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
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("숫자 리스트 안에서 자신의 2배인 수가 있는 수의 개수를 반한한다.")
    void doublesTest(List<Integer> numbers, int expected) {
        Doubles sut = Doubles.from(numbers);

        int actual = sut.count();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(List.of(1, 2), 1),
                Arguments.of(List.of(1, 3), 0),
                Arguments.of(List.of(2, 4, 8), 2),
                Arguments.of(List.of(0, 2, 4, 8), 2),
                Arguments.of(List.of(2, 4, 8, 10, 0), 2),
                Arguments.of(List.of(7, 5, 11, 13, 1, 3, 0), 0),
                Arguments.of(List.of(1, 4, 3, 2, 9, 7, 18, 22, 0), 3)
        );
    }
}

class Doubles {

    public static final int EXCLUDE_NUMBER = 0;
    public static final int HALF_LIMIT = 50;
    public static final int LIMIT = 101;
    private final List<Integer> numbers;
    private final boolean[] numberExists = new boolean[LIMIT];

    private Doubles(List<Integer> numbers) {
        this.numbers = numbers;
        initNumberExists(numbers);
    }

    public static Doubles from(List<Integer> numbers) {
        return new Doubles(numbers);
    }

    private void initNumberExists(List<Integer> numbers) {
        for (int number : numbers) {
            numberExists[number] = true;
        }
    }

    public int count() {
        return (int) numbers.stream()
                .filter(this::isDouble)
                .count();
    }

    private boolean isDouble(Integer number) {
        return number != EXCLUDE_NUMBER && number <= HALF_LIMIT && numberExists[number * 2];
    }
}
~~~
