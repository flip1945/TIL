# 양수 개수 세기 (Bronze 3)

### 문제 설명

주어진 N개의 정수 중에서 양의 정수의 개수를 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/14909

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Integer> numbers = inputNumbers(scanner.nextLine());
        PositiveIntegers positiveIntegers = PositiveIntegers.from(numbers);

        System.out.println(positiveIntegers.count());
    }

    private static List<Integer> inputNumbers(String numbers) {
        return Arrays.stream(numbers.split(" "))
                .map(Integer::parseInt)
                .collect(Collectors.toList());
    }
}

class PositiveIntegers {

    private final List<PositiveInteger> positiveIntegers;

    private PositiveIntegers(List<PositiveInteger> positiveIntegers) {
        this.positiveIntegers = positiveIntegers;
    }

    public static PositiveIntegers from(List<Integer> numbers) {
        return new PositiveIntegers(initPositiveNumbers(numbers));
    }

    private static List<PositiveInteger> initPositiveNumbers(List<Integer> numbers) {
        return numbers.stream()
                .map(PositiveInteger::new)
                .filter(PositiveInteger::isPositive)
                .collect(Collectors.toList());
    }

    public int count() {
        return positiveIntegers.size();
    }
}

class PositiveInteger {

    private final int number;

    public PositiveInteger(int number) {
        this.number = number;
    }

    public boolean isPositive() {
        return number > 0;
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
    @DisplayName("주어진 숫자 중에서 양의 정수의 개수를 반환한다.")
    void getCountOfPositiveIntegersTest(List<Integer> numbers, int expected) {
        PositiveIntegers sut = PositiveIntegers.from(numbers);

        int actual = sut.count();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(List.of(1), 1),
                Arguments.of(List.of(1, 1), 2),
                Arguments.of(List.of(1, 1, -1), 2),
                Arguments.of(List.of(1, 1, -1, 0), 2),
                Arguments.of(List.of(1, 1, -1, 0, 3, 4, 5), 5),
                Arguments.of(List.of(3, 9, 11, 32, 8, 2, 6), 7),
                Arguments.of(List.of(-2, 0, 21, 3, 8, 17, 32, -8, 7, 0), 6),
                Arguments.of(List.of(0), 0)
        );
    }
}

class PositiveIntegers {

    private final List<PositiveInteger> positiveIntegers;

    private PositiveIntegers(List<PositiveInteger> positiveIntegers) {
        this.positiveIntegers = positiveIntegers;
    }

    public static PositiveIntegers from(List<Integer> numbers) {
        return new PositiveIntegers(initPositiveNumbers(numbers));
    }

    private static List<PositiveInteger> initPositiveNumbers(List<Integer> numbers) {
        return numbers.stream()
                .map(PositiveInteger::new)
                .filter(PositiveInteger::isPositive)
                .collect(Collectors.toList());
    }

    public int count() {
        return positiveIntegers.size();
    }
}

class PositiveInteger {

    private final int number;

    public PositiveInteger(int number) {
        this.number = number;
    }

    public boolean isPositive() {
        return number > 0;
    }
}
~~~
