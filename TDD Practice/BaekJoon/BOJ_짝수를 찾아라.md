# 짝수를 찾아라 (Bronze 3)

### 문제 설명

7개의 자연수가 주어질 때, 이들 중 짝수인 자연수들을 모두 골라 그 합을 구하고, 고른 짝수들 중 최솟값을 찾는 프로그램을 작성하시오.

예를 들어, 7개의 자연수 13, 78, 39, 42, 54, 93, 86가 주어지면 이들 중 짝수는 78, 42, 54, 86이므로 그 합은 78 + 42 + 54 + 86 = 260 이 되고, 42 < 54 < 78 < 86 이므로 짝수들 중 최솟값은 42가 된다.

출처 : https://www.acmicpc.net/problem/3058

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            List<Integer> number = inputNumbers(scanner);
            EvenNumbers evenNumbers = new EvenNumbers(number);
            printSumOfEvenNumbersAndMinEvenNumber(evenNumbers);
        }
    }

    private static void printSumOfEvenNumbersAndMinEvenNumber(EvenNumbers evenNumbers) {
        System.out.println(evenNumbers.sum() + " " + evenNumbers.getMin());
    }

    private static List<Integer> inputNumbers(Scanner scanner) {
        return IntStream.range(0, 7).mapToObj(j -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class EvenNumbers {

    private final List<EvenNumber> evenNumbers;

    public EvenNumbers(List<Integer> numbers) {
        this.evenNumbers = initEvenNumbers(numbers);
    }

    private List<EvenNumber> initEvenNumbers(List<Integer> numbers) {
        return numbers.stream()
                .map(EvenNumber::new)
                .filter(EvenNumber::isEven)
                .collect(Collectors.toList());
    }

    public int sum() {
        return evenNumbers.stream()
                .mapToInt(EvenNumber::getEvenNumber)
                .sum();
    }

    public int getMin() {
        return evenNumbers.stream()
                .sorted()
                .findFirst()
                .orElseThrow()
                .getEvenNumber();
    }
}

class EvenNumber implements Comparable<EvenNumber> {

    private final int evenNumber;

    public EvenNumber(int evenNumber) {
        this.evenNumber = evenNumber;
    }

    public boolean isEven() {
        return evenNumber % 2 == 0;
    }

    public int getEvenNumber() {
        return evenNumber;
    }

    @Override
    public int compareTo(EvenNumber o) {
        return Integer.compare(this.evenNumber, o.evenNumber);
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
    @MethodSource("provideNumbersAndSumOfEvenNumbers")
    @DisplayName("짝수의 합을 반환한다.")
    void sumOfEvenNumbersTest(List<Integer> numbers, int expected) {
        EvenNumbers sut = new EvenNumbers(numbers);

        int actual = sut.sum();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbersAndSumOfEvenNumbers() {
        return Stream.of(
                Arguments.of(List.of(1, 2, 3, 4, 5, 6, 7), 12),
                Arguments.of(List.of(2, 2, 2, 2, 2, 2, 2), 14),
                Arguments.of(List.of(1, 1, 1, 1, 1, 1, 2), 2),
                Arguments.of(List.of(13, 78, 39, 42, 54, 93, 86), 260)
        );
    }

    @ParameterizedTest
    @MethodSource("provideNumbersAndMinEvenNumber")
    @DisplayName("가장 작은 짝수를 반환한다.")
    void scytalePasswordTest(List<Integer> numbers, int expected) {
        EvenNumbers sut = new EvenNumbers(numbers);

        int actual = sut.getMin();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbersAndMinEvenNumber() {
        return Stream.of(
                Arguments.of(List.of(1, 2, 3, 4, 5, 6, 7), 2),
                Arguments.of(List.of(1, 6, 1, 8, 1, 1, 5), 6),
                Arguments.of(List.of(1, 1, 1, 4, 1, 1, 4), 4),
                Arguments.of(List.of(13, 78, 39, 42, 54, 93, 86), 42)
        );
    }
}

class EvenNumbers {

    private final List<EvenNumber> evenNumbers;

    public EvenNumbers(List<Integer> numbers) {
        this.evenNumbers = initEvenNumbers(numbers);
    }

    private List<EvenNumber> initEvenNumbers(List<Integer> numbers) {
        return numbers.stream()
                .map(EvenNumber::new)
                .filter(EvenNumber::isEven)
                .collect(Collectors.toList());
    }

    public int sum() {
        return evenNumbers.stream()
                .mapToInt(EvenNumber::getEvenNumber)
                .sum();
    }

    public int getMin() {
        return evenNumbers.stream()
                .sorted()
                .findFirst()
                .orElseThrow()
                .getEvenNumber();
    }
}

class EvenNumber implements Comparable<EvenNumber> {

    private final int evenNumber;

    public EvenNumber(int evenNumber) {
        this.evenNumber = evenNumber;
    }

    public boolean isEven() {
        return evenNumber % 2 == 0;
    }

    public int getEvenNumber() {
        return evenNumber;
    }

    @Override
    public int compareTo(EvenNumber o) {
        return Integer.compare(this.evenNumber, o.evenNumber);
    }
}
~~~
