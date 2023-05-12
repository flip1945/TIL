# 수열의 변화 (Bronze 1)

### 문제 설명

크기가 N인 수열 A가 주어졌을 때, 세준이는 인접한 두 원소의 차이를 이용해서 크기가 N-1인 수열 B를 만들 수 있다.

예를 들어, A = {5, 6, 3, 9, -1} 이었을 때, B = {6-5, 3-6, 9-3, -1-9} = {1, -3, 6, -10}이 된다. 즉, B[i] = A[i+1]-A[i]가 된다.

수열 A가 주어졌을 때, 세준이가 위의 방법을 K번 했을 때 나오는 수열을 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1551

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        int n = Integer.parseInt(st.nextToken());
        int k = Integer.parseInt(st.nextToken());
        List<Integer> numbers = getNumbers(scanner.nextLine());

        AdjacentDifferenceCalculator calculator = new AdjacentDifferenceCalculator(numbers);
        System.out.println(calculator.calculateRepeat(k));
    }

    private static List<Integer> getNumbers(String numbers) {
        return Arrays.stream(numbers.split(","))
                .map(Integer::parseInt)
                .collect(Collectors.toList());
    }
}

class AdjacentDifferenceCalculator {
    private final List<Integer> numbers;

    public AdjacentDifferenceCalculator(List<Integer> numbers) {
        this.numbers = numbers;
    }

    public AdjacentDifferenceCalculator calculateRepeat(int n) {
        if (n <= 0) {
            return new AdjacentDifferenceCalculator(numbers);
        }
        return new AdjacentDifferenceCalculator(calculateAdjacentDifference()).calculateRepeat(--n);
    }

    private List<Integer> calculateAdjacentDifference() {
        return IntStream.range(0, numbers.size() - 1)
                .mapToObj(i -> numbers.get(i + 1) - numbers.get(i))
                .collect(Collectors.toList());
    }

    @Override
    public String toString() {
        return numbers.stream()
                .map(String::valueOf)
                .collect(Collectors.joining(","));
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
    @MethodSource("provideNumbersAndN")
    @DisplayName("인접한 수들의 차이를 n번 반복한 결과를 반환한다.")
    void this_returns_repeat_result_of_difference_between_adjacent_numbers(List<Integer> numbers, int n, String expected) {
        AdjacentDifferenceCalculator sut = new AdjacentDifferenceCalculator(numbers);

        AdjacentDifferenceCalculator actual = sut.calculateRepeat(n);

        assertEquals(expected, actual.toString());
    }

    private static Stream<Arguments> provideNumbersAndN() {
        return Stream.of(
                Arguments.of(List.of(1, 2, 3, 4, 5), 1, "1,1,1,1"),
                Arguments.of(List.of(1, 2, 3, 4, 5), 2, "0,0,0"),
                Arguments.of(List.of(5, 6, 3, 9, -1), 2, "-4,9,-16"),
                Arguments.of(List.of(5, 6, 3, 9, -1), 4, "-38"),
                Arguments.of(List.of(4, 4, 4, 4, 4, 4, 4, 4), 3, "0,0,0,0,0"),
                Arguments.of(List.of(-100, 100), 0, "-100,100")
        );
    }
}

class AdjacentDifferenceCalculator {
    private final List<Integer> numbers;

    public AdjacentDifferenceCalculator(List<Integer> numbers) {
        this.numbers = numbers;
    }

    public AdjacentDifferenceCalculator calculateRepeat(int n) {
        if (n <= 0) {
            return new AdjacentDifferenceCalculator(numbers);
        }
        return new AdjacentDifferenceCalculator(calculateAdjacentDifference()).calculateRepeat(--n);
    }

    private List<Integer> calculateAdjacentDifference() {
        return IntStream.range(0, numbers.size() - 1)
                .mapToObj(i -> numbers.get(i + 1) - numbers.get(i))
                .collect(Collectors.toList());
    }

    @Override
    public String toString() {
        return numbers.stream()
                .map(String::valueOf)
                .collect(Collectors.joining(","));
    }
}
~~~
