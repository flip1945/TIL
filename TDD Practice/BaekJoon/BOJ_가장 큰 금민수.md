# 가장 큰 금민수 (Bronze 1)

### 문제 설명

은민이는 4와 7을 좋아하고, 나머지 숫자는 싫어한다. 금민수는 어떤 수가 4와 7로만 이루어진 수를 말한다.

N이 주어졌을 때, N보다 작거나 같은 금민수 중 가장 큰 것을 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1526

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        OnlyFourSevenNumberChecker checker = new OnlyFourSevenNumberChecker();
        LargestOnlyFourSevenNumberFinder finder = new LargestOnlyFourSevenNumberFinder(checker);

        System.out.println(finder.find(scanner.nextInt()));
    }
}

class LargestOnlyFourSevenNumberFinder {
    private final OnlyFourSevenNumberChecker checker;

    public LargestOnlyFourSevenNumberFinder(OnlyFourSevenNumberChecker checker) {
        this.checker = checker;
    }

    public int find(int n) {
        while (n > 4) {
            if (checker.check(n)) {
                return n;
            }
            n--;
        }
        return 4;
    }
}

class OnlyFourSevenNumberChecker {
    public boolean check(int number) {
        while (number > 0) {
            if (!isFourOrSeven(number % 10)) {
                return false;
            }
            number /= 10;
        }
        return true;
    }

    private boolean isFourOrSeven(int units) {
        return units == 4 || units == 7;
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

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideN")
    @DisplayName("n 이하의 가장 큰 4와 7로만 이루어진 숫자를 반환한다.")
    void it_is_largest_four_seven_number_only_under_n(int n, int expected) {
        LargestOnlyFourSevenNumberFinder sut = new LargestOnlyFourSevenNumberFinder(new OnlyFourSevenNumberChecker());

        int actual = sut.find(n);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideN() {
        return Stream.of(
                Arguments.of(10, 7),
                Arguments.of(6, 4),
                Arguments.of(100, 77),
                Arguments.of(75, 74),
                Arguments.of(73, 47),
                Arguments.of(5, 4),
                Arguments.of(4, 4),
                Arguments.of(1000000, 777777),
                Arguments.of(474747, 474747)
        );
    }

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("4와 7로만 이루어진 숫자이다.")
    void it_is_four_seven_number_only(int number, boolean expected) {
        OnlyFourSevenNumberChecker sut = new OnlyFourSevenNumberChecker();

        boolean actual = sut.check(number);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(7, true),
                Arguments.of(10, false),
                Arguments.of(4, true),
                Arguments.of(100, false),
                Arguments.of(77, true),
                Arguments.of(44, true)
        );
    }
}

class LargestOnlyFourSevenNumberFinder {
    private final OnlyFourSevenNumberChecker checker;

    public LargestOnlyFourSevenNumberFinder(OnlyFourSevenNumberChecker checker) {
        this.checker = checker;
    }

    public int find(int n) {
        while (n > 4) {
            if (checker.check(n)) {
                return n;
            }
            n--;
        }
        return 4;
    }
}

class OnlyFourSevenNumberChecker {
    public boolean check(int number) {
        while (number > 0) {
            if (!isFourOrSeven(number % 10)) {
                return false;
            }
            number /= 10;
        }
        return true;
    }

    private boolean isFourOrSeven(int units) {
        return units == 4 || units == 7;
    }
}
~~~
