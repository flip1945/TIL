# Lucky 7 (Bronze 5)

### 문제 설명

Fact or Fiction, some people consider 7 to be a lucky digit/number.

Given a number, determine how lucky the number is by printing one of four values:

* Print 0 if the number does not contain 7 and is not divisible by 7.
* Print 1 if the number does not contain 7 but is divisible by 7.
* Print 2 if the number does contain 7 but is not divisible by 7.
* Print 3 if the number does contain 7 and is divisible by 7.

출처 : https://www.acmicpc.net/problem/30224

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int number = scanner.nextInt();
        LuckySeven luckySeven = new LuckySeven(number);

        System.out.println(luckySeven.getType());
    }
}

class LuckySeven {

    private final int number;

    public LuckySeven(int number) {
        this.number = number;
    }

    public int getType() {
        return LuckySevenType.from(number).getType();
    }
}

enum LuckySevenType {
    ZERO(0, number -> false),
    ONE(1, number -> !containsSeven(number) && divisibleBySeven(number)),
    TWO(2, number -> containsSeven(number) && !divisibleBySeven(number)),
    THREE(3, number -> containsSeven(number) && divisibleBySeven(number));

    private static final String SEVEN_STRING = "7";
    private static final int SEVEN_NUMBER = 7;

    private final int type;
    private final Predicate<Integer> filter;

    LuckySevenType(int type, Predicate<Integer> filter) {
        this.type = type;
        this.filter = filter;
    }

    public int getType() {
        return type;
    }

    public static LuckySevenType from(int number) {
        for (LuckySevenType type : values()) {
            if (type.filter.test(number)) {
                return type;
            }
        }
        return LuckySevenType.ZERO;
    }

    private static boolean containsSeven(int number) {
        return String.valueOf(number).contains(SEVEN_STRING);
    }

    private static boolean divisibleBySeven(int number) {
        return number % SEVEN_NUMBER == 0;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

import java.util.function.Predicate;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @ValueSource(ints = {1, 1000000000})
    @DisplayName("주어진 숫자가 7을 포함하지 않고 7로 나눠지지 않는다면 0을 반환한다.")
    void luckySevenReturnsZeroIfTheNumberDoesNotContainSevenAndIsNotDivisibleBySeven(int number) {
        LuckySeven sut = new LuckySeven(number);

        int actual = sut.getType();

        assertEquals(0, actual);
    }

    @ParameterizedTest
    @ValueSource(ints = {14, 21})
    @DisplayName("주어진 숫자가 7을 포함하지 않고 7로 나눠진다면 1을 반환한다.")
    void luckySevenReturnsOneIfTheNumberDoesNotContainSevenAndIsDivisibleBySeven(int number) {
        LuckySeven sut = new LuckySeven(number);

        int actual = sut.getType();

        assertEquals(1, actual);
    }

    @ParameterizedTest
    @ValueSource(ints = {17, 107})
    @DisplayName("주어진 숫자가 7을 포함하고 7로 나눠지지 않는다면 2을 반환한다.")
    void luckySevenReturnsTwoIfTheNumberContainsSevenAndIsNotDivisibleBySeven(int number) {
        LuckySeven sut = new LuckySeven(number);

        int actual = sut.getType();

        assertEquals(2, actual);
    }

    @ParameterizedTest
    @ValueSource(ints = {7, 77, 70})
    @DisplayName("주어진 숫자가 7을 포함하고 7로 나눠진다면 3을 반환한다.")
    void luckySevenReturnsThreeIfTheNumberContainsSevenAndIsDivisibleBySeven(int number) {
        LuckySeven sut = new LuckySeven(number);

        int actual = sut.getType();

        assertEquals(3, actual);
    }
}

class LuckySeven {

    private final int number;

    public LuckySeven(int number) {
        this.number = number;
    }

    public int getType() {
        return LuckySevenType.from(number).getType();
    }
}

enum LuckySevenType {
    ZERO(0, number -> false),
    ONE(1, number -> !containsSeven(number) && divisibleBySeven(number)),
    TWO(2, number -> containsSeven(number) && !divisibleBySeven(number)),
    THREE(3, number -> containsSeven(number) && divisibleBySeven(number));

    private static final String SEVEN_STRING = "7";
    private static final int SEVEN_NUMBER = 7;

    private final int type;
    private final Predicate<Integer> filter;

    LuckySevenType(int type, Predicate<Integer> filter) {
        this.type = type;
        this.filter = filter;
    }

    public int getType() {
        return type;
    }

    public static LuckySevenType from(int number) {
        for (LuckySevenType type : values()) {
            if (type.filter.test(number)) {
                return type;
            }
        }
        return LuckySevenType.ZERO;
    }

    private static boolean containsSeven(int number) {
        return String.valueOf(number).contains(SEVEN_STRING);
    }

    private static boolean divisibleBySeven(int number) {
        return number % SEVEN_NUMBER == 0;
    }
}
~~~
