# 신용카드 판별 (Bronze 2)

### 문제 설명

신용카드는 총 16자리의 숫자로 구성되어 있다. 언뜻 보기에는 무작위로 된 숫자로 구성되어 있는 것 같이 보이지만 그 속에는 하나의 수학적 비밀이 숨겨져 있다. 그중 하나가 카드 번호가 유효 한지 유효하지 않은 지 검사하는 Luhn 공식이다. 그 공식은 다음과 같다.

1. 신용카드의 16자리 숫자에서 맨 우측 수부터 세어 홀수 번째 수는 그대로 두고, 짝수 번째 수를 2배로 만든다.
2. 2배로 만든 짝수 번째 수가 10 이상인 경우, 각 자리의 숫자를 더하고 그 수로 대체한다.
3. 이와 같이 얻은 모든 자리의 수를 더한다.
4. 그 합이 10으로 나뉘면 “정당한 번호”(유효)이고 그렇지 않으면 “부당한 번호”(유효하지 않음)로 판정된다.

다음 공식을 이용해 주어진 신용카드의 번호가 유효한지, 유효하지 않은 지 판단해라.

출처 : https://www.acmicpc.net/problem/14726

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            CardNumber cardNumber = new CardNumber(scanner.nextLine());
            System.out.println(cardNumber.isValid());
        }
    }
}

class CardNumber {

    public static final String TRUE_STRING = "T";
    public static final String FALSE_STRING = "F";
    public static final int CARD_NUMBER_SIZE = 16;
    public static final int VALID_DIVISOR = 10;
    public static final int VALID_REMAINDER = 0;

    private final String cardNumber;

    public CardNumber(String cardNumber) {
        this.cardNumber = cardNumber;
    }

    public String isValid() {
        return checkValidCardNumber(cardNumber) ? TRUE_STRING : FALSE_STRING;
    }

    private boolean checkValidCardNumber(String cardNumber) {
        return getSumOfDigits(cardNumber) % VALID_DIVISOR == VALID_REMAINDER;
    }

    private int getSumOfDigits(String cardNumber) {
        return IntStream.range(0, CARD_NUMBER_SIZE)
                .mapToObj(i -> new Digits(Character.getNumericValue(cardNumber.charAt(i)), i))
                .mapToInt(Digits::getNumber)
                .sum();
    }
}

class Digits {

    private final int digits;
    private final DigitType type;

    public Digits(int digits, int index) {
        this.digits = digits;
        this.type = initDigitType(index);
    }

    private DigitType initDigitType(int index) {
        return index % 2 == 0 ? DigitType.Even : DigitType.Odd;
    }

    public int getNumber() {
        if (isEvenDigit()) {
            return getDigitOfEven();
        }
        return digits;
    }

    private boolean isEvenDigit() {
        return type.equals(DigitType.Even);
    }

    private int getDigitOfEven() {
        int number = digits * 2;
        if (number >= 10) {
            return number / 10 + number % 10;
        }
        return number;
    }

    enum DigitType {
        Even, Odd
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
    @MethodSource("provideCardNumbers")
    @DisplayName("신용카드 번호가 유효하다면 T 아니라면 F를 반환한다.")
    void isValidCardNumberTest(String cardNumber, String expected) {
        CardNumber sut = new CardNumber(cardNumber);

        String actual = sut.isValid();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCardNumbers() {
        return Stream.of(
                Arguments.of("0000000000000000", "T"),
                Arguments.of("0000000000000001", "F"),
                Arguments.of("0000000000000505", "T"),
                Arguments.of("0000000000008060", "T"),
                Arguments.of("2720992711828767", "T"),
                Arguments.of("3444063910462763", "F"),
                Arguments.of("6011733895106094", "T")
        );
    }
}

class CardNumber {

    public static final String TRUE_STRING = "T";
    public static final String FALSE_STRING = "F";
    public static final int CARD_NUMBER_SIZE = 16;
    public static final int VALID_DIVISOR = 10;
    public static final int VALID_REMAINDER = 0;

    private final String cardNumber;

    public CardNumber(String cardNumber) {
        this.cardNumber = cardNumber;
    }

    public String isValid() {
        return checkValidCardNumber(cardNumber) ? TRUE_STRING : FALSE_STRING;
    }

    private boolean checkValidCardNumber(String cardNumber) {
        return getSumOfDigits(cardNumber) % VALID_DIVISOR == VALID_REMAINDER;
    }

    private int getSumOfDigits(String cardNumber) {
        return IntStream.range(0, CARD_NUMBER_SIZE)
                .mapToObj(i -> new Digits(Character.getNumericValue(cardNumber.charAt(i)), i))
                .mapToInt(Digits::getNumber)
                .sum();
    }
}

class Digits {

    private final int digits;
    private final DigitType type;

    public Digits(int digits, int index) {
        this.digits = digits;
        this.type = initDigitType(index);
    }

    private DigitType initDigitType(int index) {
        return index % 2 == 0 ? DigitType.Even : DigitType.Odd;
    }

    public int getNumber() {
        if (isEvenDigit()) {
            return getDigitOfEven();
        }
        return digits;
    }

    private boolean isEvenDigit() {
        return type.equals(DigitType.Even);
    }

    private int getDigitOfEven() {
        int number = digits * 2;
        if (number >= 10) {
            return number / 10 + number % 10;
        }
        return number;
    }

    enum DigitType {
        Even, Odd
    }
}
~~~
