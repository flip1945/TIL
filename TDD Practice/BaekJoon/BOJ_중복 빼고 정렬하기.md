# 중복 빼고 정렬하기 (Silver 5)

### 문제 설명

N개의 정수가 주어진다. 이때, N개의 정수를 오름차순으로 정렬하는 프로그램을 작성하시오. 같은 정수는 한 번만 출력한다.

출처 : https://www.acmicpc.net/problem/10867

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();

        SortedDistinctNumbers sortedDistinctNumbers = SortedDistinctNumbers.from(scanner.nextLine());
        System.out.println(sortedDistinctNumbers);
    }
}

class SortedDistinctNumbers {

    public static final String DELIMITER = " ";

    private final List<Integer> numbers;

    public SortedDistinctNumbers(List<Integer> numbers) {
        this.numbers = numbers;
    }

    public static SortedDistinctNumbers from(String numbers) {
        return new SortedDistinctNumbers(initNumbers(numbers));
    }

    private static List<Integer> initNumbers(String numbers) {
        return Arrays.stream(numbers.split(DELIMITER))
                .map(Integer::parseInt)
                .collect(Collectors.toList());
    }

    @Override
    public String toString() {
        return String.join(DELIMITER, getSortedDistinctNumbers());
    }

    public List<String> getSortedDistinctNumbers() {
        return numbers.stream()
                .sorted()
                .distinct()
                .map(String::valueOf)
                .collect(Collectors.toList());
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
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("주어진 숫자에서 중복을 제거하고 오름차순으로 정렬한 결과를 반환한다.")
    void sortAdnDistinctTest(String numbers, String expected) {
        SortedDistinctNumbers sut = SortedDistinctNumbers.from(numbers);

        String actual = sut.toString();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of("1", "1"),
                Arguments.of("1 2", "1 2"),
                Arguments.of("1 3 2", "1 2 3"),
                Arguments.of("1 1 3 2", "1 2 3"),
                Arguments.of("1 4 2 3 1 4 2 3 1 2", "1 2 3 4"),
                Arguments.of("5 5 4 4 3 2 1 1", "1 2 3 4 5")
        );
    }
}

class SortedDistinctNumbers {

    public static final String DELIMITER = " ";

    private final List<Integer> numbers;

    public SortedDistinctNumbers(List<Integer> numbers) {
        this.numbers = numbers;
    }

    public static SortedDistinctNumbers from(String numbers) {
        return new SortedDistinctNumbers(initNumbers(numbers));
    }

    private static List<Integer> initNumbers(String numbers) {
        return Arrays.stream(numbers.split(DELIMITER))
                .map(Integer::parseInt)
                .collect(Collectors.toList());
    }

    @Override
    public String toString() {
        return String.join(DELIMITER, getSortedDistinctNumbers());
    }

    public List<String> getSortedDistinctNumbers() {
        return numbers.stream()
                .sorted()
                .distinct()
                .map(String::valueOf)
                .collect(Collectors.toList());
    }
}
~~~
