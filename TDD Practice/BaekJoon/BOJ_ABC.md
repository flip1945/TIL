# ABC (Bronze 3)

### 문제 설명

세 수 A, B, C가 주어진다. A는 B보다 작고, B는 C보다 작다.

세 수 A, B, C가 주어졌을 때, 입력에서 주어진 순서대로 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/3047

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Abc abc = new Abc(scanner.nextLine(), scanner.nextLine());
        System.out.println(abc.getNumbersByAbcOrders());
    }
}

class Abc {

    private static final String DELIMITER = " ";
    private static final int A_INDEX = 0;
    private static final int B_INDEX = 1;
    private static final int C_INDEX = 2;

    private final String numbers;
    private final String abcOrders;

    public Abc(String numbers, String abcOrders) {
        this.numbers = numbers;
        this.abcOrders = abcOrders;
    }

    public String getNumbersByAbcOrders() {
        return getNumbersByAbcOrders(abcOrders);
    }

    private String getNumbersByAbcOrders(String abcOrders) {
        return Arrays.stream(abcOrders.split(""))
                .map(this::getAbcNumber)
                .collect(Collectors.joining())
                .trim();
    }

    private String getAbcNumber(String abc) {
        List<Integer> sortedNumber = getSortedNumbers();
        switch (abc) {
            case "A":
                return sortedNumber.get(A_INDEX) + DELIMITER;
            case "B":
                return sortedNumber.get(B_INDEX) + DELIMITER;
            case "C":
                return sortedNumber.get(C_INDEX) + DELIMITER;
            default:
                throw new IllegalArgumentException();
        }
    }

    private List<Integer> getSortedNumbers() {
        return Arrays.stream(numbers.split(DELIMITER))
                .map(Integer::parseInt)
                .sorted()
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
    @MethodSource("provideNumbersAndAbcOrders")
    @DisplayName("세 수 A, B, C가 주어졌을 때, 입력에서 주어진 순서대로 반환한다.")
    void returnNumbersInOrderGivenByAbcOrders(String numbers, String abcOrders, String expected) {
        Abc sut = new Abc(numbers, abcOrders);

        String actual = sut.getNumbersByAbcOrders();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbersAndAbcOrders() {
        return Stream.of(
                Arguments.of("1 2 3", "ABC", "1 2 3"),
                Arguments.of("1 2 5", "ABC", "1 2 5"),
                Arguments.of("1 2 5", "ACB", "1 5 2"),
                Arguments.of("1 2 5", "CBA", "5 2 1"),
                Arguments.of("1 5 3", "ABC", "1 3 5"),
                Arguments.of("6 4 2", "CAB", "6 2 4")
        );
    }
}

class Abc {

    private static final String DELIMITER = " ";
    private static final int A_INDEX = 0;
    private static final int B_INDEX = 1;
    private static final int C_INDEX = 2;

    private final String numbers;
    private final String abcOrders;

    public Abc(String numbers, String abcOrders) {
        this.numbers = numbers;
        this.abcOrders = abcOrders;
    }

    public String getNumbersByAbcOrders() {
        return getNumbersByAbcOrders(abcOrders);
    }

    private String getNumbersByAbcOrders(String abcOrders) {
        return Arrays.stream(abcOrders.split(""))
                .map(this::getAbcNumber)
                .collect(Collectors.joining())
                .trim();
    }

    private String getAbcNumber(String abc) {
        List<Integer> sortedNumber = getSortedNumbers();
        switch (abc) {
            case "A":
                return sortedNumber.get(A_INDEX) + DELIMITER;
            case "B":
                return sortedNumber.get(B_INDEX) + DELIMITER;
            case "C":
                return sortedNumber.get(C_INDEX) + DELIMITER;
            default:
                throw new IllegalArgumentException();
        }
    }

    private List<Integer> getSortedNumbers() {
        return Arrays.stream(numbers.split(DELIMITER))
                .map(Integer::parseInt)
                .sorted()
                .collect(Collectors.toList());
    }
}
~~~
