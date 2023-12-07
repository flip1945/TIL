# Oddities (Bronze 4)

### 문제 설명

Some numbers are just, well, odd. For example, the number 3 is odd, because it is not a multiple of two. Numbers that are a multiple of two are not odd, they are even. More precisely, if a number n can be expressed as n = 2 ∗ k for some integer k, then n is even. For example, 6 = 2 ∗ 3 is even.

Some people get confused about whether numbers are odd or even. To see a common example, do an internet search for the query “is zero even or odd?” (Don’t search for this now! You have a problem to solve!)

Write a program to help these confused people.

출처 : https://www.acmicpc.net/problem/10480

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Oddities oddities = new Oddities();
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            System.out.println(oddities.determineOddOrEven(scanner.nextInt()));
        }
    }
}

class Oddities {

    private static final String EVEN_SUFFIX = " is even";
    private static final String ODD_SUFFIX = " is odd";

    public String determineOddOrEven(int number) {
        return isEvenNumber(number) ? number + EVEN_SUFFIX : number + ODD_SUFFIX;
    }

    private boolean isEvenNumber(int number) {
        return number % 2 == 0;
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

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumber")
    @DisplayName("숫자가 주어지면 홀수 짝수인지 판별한다.")
    void odditiesDeterminesWhetherItIsAnOddEvenNumber(int number, String expected) {
        Oddities sut = new Oddities();

        String actual = sut.determineOddOrEven(number);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideNumber() {
        return Stream.of(
                Arguments.of(1, "1 is odd"),
                Arguments.of(3, "3 is odd"),
                Arguments.of(0, "0 is even"),
                Arguments.of(10, "10 is even"),
                Arguments.of(-5, "-5 is odd")
        );
    }
}

class Oddities {

    private static final String EVEN_SUFFIX = " is even";
    private static final String ODD_SUFFIX = " is odd";

    public String determineOddOrEven(int number) {
        return isEvenNumber(number) ? number + EVEN_SUFFIX : number + ODD_SUFFIX;
    }

    private boolean isEvenNumber(int number) {
        return number % 2 == 0;
    }
}
~~~
