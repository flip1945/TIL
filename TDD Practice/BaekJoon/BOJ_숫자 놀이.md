# 숫자 놀이 (Bronze 2)

### 문제 설명

초등학생인 도겸이는 숫자를 좋아한다. 어느 날 도겸이는 숫자 책을 보다가 간단한 놀이를 하나 생각해냈다. 숫자 놀이의 규칙은 다음과 같다.

1. 주어진 숫자의 각 자릿수를 더한다.
2. 결과가 한 자릿수가 될 때 까지 규칙1을 반복한다.

예를들어, 숫자 673에 규칙을 적용해보면 결과는 7이 된다 ; 6 + 7 + 3 = 16, 1 + 6 = 7 

도겸이는 당신과 함께 숫자놀이를 하고싶어한다. 도겸이가 주는 숫자들을 풀어보자.

출처 : https://www.acmicpc.net/problem/2145

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = inputNumber(scanner);
        while (n != 0) {
            NumberGame numberGame = new NumberGame(n);
            System.out.println(numberGame.getAnswer());

            n = inputNumber(scanner);
        }
    }

    private static int inputNumber(Scanner scanner) {
        return Integer.parseInt(scanner.nextLine());
    }
}

class NumberGame {

    private final int number;

    public NumberGame(int number) {
        this.number = number;
    }

    public int getAnswer() {
        return next(number).number;
    }

    private NumberGame next(int number) {
        if (number < 10) {
            return new NumberGame(number);
        }
        return next(sumOfDigits(number));
    }

    private int sumOfDigits(int number) {
        int result = 0;
        while (number > 0) {
            result += number % 10;
            number /= 10;
        }
        return result;
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
    @MethodSource("provideNumbers")
    @DisplayName("숫자 게임의 결과를 반환한다.")
    void numberGameTest(int number, int expected) {
        NumberGame sut = new NumberGame(number);

        int actual = sut.getAnswer();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of(11, 2),
                Arguments.of(10, 1),
                Arguments.of(55, 1),
                Arguments.of(123, 6),
                Arguments.of(555, 6),
                Arguments.of(999, 9),
                Arguments.of(1000, 1),
                Arguments.of(673, 7)
        );
    }
}

class NumberGame {

    private final int number;

    public NumberGame(int number) {
        this.number = number;
    }

    public int getAnswer() {
        return next(number).number;
    }

    private NumberGame next(int number) {
        if (number < 10) {
            return new NumberGame(number);
        }
        return next(sumOfDigits(number));
    }

    private int sumOfDigits(int number) {
        int result = 0;
        while (number > 0) {
            result += number % 10;
            number /= 10;
        }
        return result;
    }
}
~~~
