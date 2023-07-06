# 분수좋아해 (Bronze 3)

### 문제 설명

당신은 학생들의 기초수학 학습을 돕는 소프트웨어를 개발하는 팀의 개발자이다. 당신은 가분수를 대분수(?)로 출력하는 부분을 개발해야 한다. 진분수는 분자가 분모보다 작은 분수이다; 대분수는 정수부를 따로 떼어주고 남는 부분을 진분수로 쓰는 기법이다. 예제로, 27/12는 대분수로 2 3/12이다. 기약분수로 만들지 말아야 한다.(3/12를 1/4로 바꿔 출력하지 마시오.)

출처 : https://www.acmicpc.net/problem/10474

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int numerator = scanner.nextInt();
        int denominator = scanner.nextInt();

        while (numerator != 0 || denominator != 0) {
            System.out.println(MixedFraction.of(numerator, denominator));
            numerator = scanner.nextInt();
            denominator = scanner.nextInt();
        }
    }
}

class MixedFraction {

    private final int wholeNumber;
    private final int numerator;
    private final int denominator;

    private MixedFraction(int numerator, int denominator) {
        this.wholeNumber = calculateWholeNumber(numerator, denominator);
        this.numerator = calculateNumerator(numerator, denominator);
        this.denominator = denominator;
    }

    private int calculateNumerator(int numerator, int denominator) {
        return numerator % denominator;
    }

    private int calculateWholeNumber(int numerator, int denominator) {
        return numerator / denominator;
    }

    public static MixedFraction of(int numerator, int denominator) {
        return new MixedFraction(numerator, denominator);
    }

    @Override
    public String toString() {
        return wholeNumber + " " + numerator + " / " + denominator;
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

import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideFractions")
    @DisplayName("주어진 분수에 대해 맞는 대분수를 문자열로 반환한다.")
    void getMixedFractionTest(int numerator, int denominator, String expected) {
        MixedFraction sut = MixedFraction.of(numerator, denominator);

        String actual = sut.toString();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideFractions() {
        return Stream.of(
                Arguments.of(3, 2, "1 1 / 2"),
                Arguments.of(5, 2, "2 1 / 2"),
                Arguments.of(2, 2, "1 0 / 2"),
                Arguments.of(1, 2, "0 1 / 2"),
                Arguments.of(27, 12, "2 3 / 12"),
                Arguments.of(2460000, 98400, "25 0 / 98400"),
                Arguments.of(3, 4000, "0 3 / 4000")
        );
    }
}

class MixedFraction {

    private final int wholeNumber;
    private final int numerator;
    private final int denominator;

    private MixedFraction(int numerator, int denominator) {
        this.wholeNumber = calculateWholeNumber(numerator, denominator);
        this.numerator = calculateNumerator(numerator, denominator);
        this.denominator = denominator;
    }

    private int calculateNumerator(int numerator, int denominator) {
        return numerator % denominator;
    }

    private int calculateWholeNumber(int numerator, int denominator) {
        return numerator / denominator;
    }

    public static MixedFraction of(int numerator, int denominator) {
        return new MixedFraction(numerator, denominator);
    }

    @Override
    public String toString() {
        return wholeNumber + " " + numerator + " / " + denominator;
    }
}
~~~
