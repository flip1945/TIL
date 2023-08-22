# 최소, 최대 2 (Bronze 3)

### 문제 설명

N개의 정수가 주어진다. 이때, 최솟값과 최댓값을 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/20053

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < t; i++) {
            int n = Integer.parseInt(scanner.nextLine());

            Numbers numbers = new Numbers(inputNumbers(scanner, n));
            System.out.println(numbers.getMinAndMax());
        }
    }

    private static List<Integer> inputNumbers(Scanner scanner, int n) {
        return Arrays.stream(scanner.nextLine().split(" "))
                .map(Integer::parseInt)
                .collect(Collectors.toList());
    }
}

class Numbers {

    public static final String DELIMITER = " ";

    private final List<Integer> numbers;

    public Numbers(List<Integer> numbers) {
        this.numbers = new ArrayList<>(numbers);
    }

    public String getMinAndMax() {
        return getMinNumber() + DELIMITER + getMaxNumber();
    }

    private int getMinNumber() {
        return numbers.stream()
                .mapToInt(Integer::intValue)
                .min()
                .orElse(Integer.MAX_VALUE);
    }

    private int getMaxNumber() {
        return numbers.stream()
                .mapToInt(Integer::intValue)
                .max()
                .orElse(Integer.MIN_VALUE);
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

import java.util.ArrayList;
import java.util.List;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("주어진 숫자들의 최소값과 최댓값을 반환한다.")
    void shouldReturnMinNumberAndMaxNumber(List<Integer> numbers, String expected) {
        Numbers sut = new Numbers(numbers);

        String actual = sut.getMinAndMax();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(List.of(1), "1 1"),
                Arguments.of(List.of(2), "2 2"),
                Arguments.of(List.of(1, 2), "1 2"),
                Arguments.of(List.of(2, 1), "1 2"),
                Arguments.of(List.of(20, 28, 22, 25, 21), "20 28"),
                Arguments.of(List.of(30, 21, 17, 25, 29), "17 30"),
                Arguments.of(List.of(20, 10, 35, 30, 7), "7 35")
        );
    }
}

class Numbers {

    public static final String DELIMITER = " ";

    private final List<Integer> numbers;

    public Numbers(List<Integer> numbers) {
        this.numbers = new ArrayList<>(numbers);
    }

    public String getMinAndMax() {
        return getMinNumber() + DELIMITER + getMaxNumber();
    }

    private int getMinNumber() {
        return numbers.stream()
                .mapToInt(Integer::intValue)
                .min()
                .orElse(Integer.MAX_VALUE);
    }

    private int getMaxNumber() {
        return numbers.stream()
                .mapToInt(Integer::intValue)
                .max()
                .orElse(Integer.MIN_VALUE);
    }
}
~~~
