# Max (Bronze 2)

### 문제 설명

In a sequence of integers, the number of times an integer occurs is called the frequency of the integer.  Given a sequence of integers, find the highest frequency.

출처 : https://www.acmicpc.net/problem/9913

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        MaxFrequency maxFrequency = new MaxFrequency(inputNumbers(scanner, n));

        System.out.println(maxFrequency.countOfMaxFrequencyNumber());
    }

    private static List<Integer> inputNumbers(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class MaxFrequency {

    private final List<Integer> numbers;

    public MaxFrequency(List<Integer> numbers) {
        this.numbers = numbers;
    }

    public int countOfMaxFrequencyNumber() {
        return getFrequencies().values()
                .stream()
                .max(Comparator.naturalOrder())
                .map(Long::intValue)
                .orElse(Integer.MIN_VALUE);
    }

    private Map<Integer, Long> getFrequencies() {
        return numbers.stream()
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
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

import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("가장 많이 등장한 숫자의 개수를 반환한다.")
    void maxFrequencyReturnsCountOfMaxFrequencyNumber(List<Integer> numbers, int expected) {
        MaxFrequency sut = new MaxFrequency(numbers);

        int actual = sut.countOfMaxFrequencyNumber();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(List.of(1, 2, 3), 1),
                Arguments.of(List.of(1, 2, 3, 1), 2),
                Arguments.of(List.of(1, 2, 5, 6, 3, 7, 11, 345, 754, 2, 5, 2), 3)
        );
    }
}

class MaxFrequency {

    private final List<Integer> numbers;

    public MaxFrequency(List<Integer> numbers) {
        this.numbers = numbers;
    }

    public int countOfMaxFrequencyNumber() {
        return getFrequencies().values()
                .stream()
                .max(Comparator.naturalOrder())
                .map(Long::intValue)
                .orElse(Integer.MIN_VALUE);
    }

    private Map<Integer, Long> getFrequencies() {
        return numbers.stream()
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    }
}
~~~
