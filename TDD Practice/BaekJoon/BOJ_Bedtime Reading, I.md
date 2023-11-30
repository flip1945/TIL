# Bedtime Reading, I (Bronze 3)

### 문제 설명

Farmer John was performing his nightly bedtime reading duties for Bessie. "Oh, this gives me a headache," moaned Bessie. "But it's just simple number theory," replied FJ. "Let's go over it again. The sigma function of a number is just the sum of the divisors of the number. So, for a number like 12, the divisors are 1, 2, 3, 4, 6, and 12. Summing them we get 28." "That's all there is to it?" asked Bessie. "Yep." replied FJ. "Perhaps someone will write a program to calculate the sigma function of an integer I (1 <= I <= 1,000,000)."

출처 : https://www.acmicpc.net/problem/14782

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Divisors divisors = new Divisors(scanner.nextInt());
        System.out.println(divisors.sum());
    }
}

class Divisors {

    private static final int START_NUMBER = 1;

    private final int number;

    public Divisors(int number) {
        this.number = number;
    }

    public int sum() {
        return getDivisors().stream()
                .mapToInt(Integer::intValue)
                .sum();
    }

    private List<Integer> getDivisors() {
        return IntStream.rangeClosed(START_NUMBER, number)
                .filter(this::isDivisor)
                .boxed()
                .collect(Collectors.toList());
    }

    private boolean isDivisor(int i) {
        return number % i == 0;
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
    @DisplayName("숫자가 주어지면 약수의 합을 구한다.")
    void divisorsReturnsSumOfDivisorGivenNumber(int number, int expected) {
        Divisors sut = new Divisors(number);

        int actual = sut.sum();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideNumber() {
        return Stream.of(
                Arguments.of(6, 12),
                Arguments.of(12, 28),
                Arguments.of(20, 42)
        );
    }
}

class Divisors {

    private static final int START_NUMBER = 1;

    private final int number;

    public Divisors(int number) {
        this.number = number;
    }

    public int sum() {
        return getDivisors().stream()
                .mapToInt(Integer::intValue)
                .sum();
    }

    private List<Integer> getDivisors() {
        return IntStream.rangeClosed(START_NUMBER, number)
                .filter(this::isDivisor)
                .boxed()
                .collect(Collectors.toList());
    }

    private boolean isDivisor(int i) {
        return number % i == 0;
    }
}
~~~
