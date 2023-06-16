# 사칙연산 (Bronze 2)

### 문제 설명

사칙연산은 덧셈, 뺄셈, 곱셈, 나눗셈으로 이루어져 있으며, 컴퓨터 프로그램에서 이를 표현하는 기호는 +, -, *, / 와 같다. 아래는 컴퓨터 프로그램에서 표현한 사칙 연산의 예제이다.

3 * 2 = 6

문제와 답이 주어졌을 때, 이를 계산하여 올바른 수식인지 계산하는 프로그램을 만들려고 한다. 만약 주어진 데이터가 3 * 2 = 6 이라면 정답으로, 3 * 2 = 7 이면 오답으로 채점을 하면 된다. 문제와 답이 주어졌을 때, 이를 채점하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/13420

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.ToLongBiFunction;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < t; i++) {
            ExpressionChecker checker = new ExpressionChecker(scanner.nextLine());
            System.out.println(checker.check());
        }
    }
}

class ExpressionChecker {

    public static final String DELIMITER = " = ";
    public static final int EXPRESSION_INDEX = 0;
    public static final int ANSWER_INDEX = 1;
    public static final String CORRECT_WORD = "correct";
    public static final String INCORRECT_WORD = "wrong answer";
    private final Expression expression;
    private final long answer;

    public ExpressionChecker(String expression) {
        String[] splitExpression = expression.split(DELIMITER);
        this.expression = new Expression(splitExpression[EXPRESSION_INDEX]);
        this.answer = Long.parseLong(splitExpression[ANSWER_INDEX]);
    }

    public String check() {
        return (expression.calculate() == answer) ? CORRECT_WORD : INCORRECT_WORD;
    }
}

enum Sign {
    PLUS("+", (firstNumber, secondNumber) -> firstNumber + secondNumber),
    MULTIPLY("*", (firstNumber, secondNumber) -> firstNumber * secondNumber),
    MINUS("-", (firstNumber, secondNumber) -> firstNumber - secondNumber),
    DIVIDE("/", (firstNumber, secondNumber) -> firstNumber / secondNumber);

    private final String sign;
    private final ToLongBiFunction<Long, Long> formula;

    Sign(String sign, ToLongBiFunction<Long, Long> formula) {
        this.sign = sign;
        this.formula = formula;
    }

    public static Sign valueOfSign(String signString) {
        return Arrays.stream(values())
                .filter(s -> s.sign.equals(signString))
                .findFirst()
                .orElseThrow();
    }

    public Long apply(long firstNumber, long secondNumber) {
        return formula.applyAsLong(firstNumber, secondNumber);
    }
}

class Expression {

    public static final String DELIMITER = " ";
    public static final int FIRST_NUMBER_INDEX = 0;
    public static final int SECOND_NUMBER_INDEX = 2;
    public static final int SIGN_INDEX = 1;
    private final long firstNumber;
    private final long secondNumber;
    private final Sign sign;

    public Expression(String expression) {
        String[] splitExpression = expression.split(DELIMITER);
        this.firstNumber = Long.parseLong(splitExpression[FIRST_NUMBER_INDEX]);
        this.secondNumber = Long.parseLong(splitExpression[SECOND_NUMBER_INDEX]);
        this.sign = Sign.valueOfSign(splitExpression[SIGN_INDEX]);
    }

    public long calculate() {
        return sign.apply(firstNumber, secondNumber);
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

import java.util.Arrays;
import java.util.function.ToLongBiFunction;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideExpressions")
    @DisplayName("수식을 계산한 결과를 반환한다.")
    void calculateTest(String expression, long expected) {
        Expression sut = new Expression(expression);

        long actual = sut.calculate();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideExpressions() {
        return Stream.of(
                Arguments.of("0 + 0", 0L),
                Arguments.of("1 + 2", 3L),
                Arguments.of("1 + 3", 4L),
                Arguments.of("2 + 3", 5L),
                Arguments.of("0 * 0", 0L),
                Arguments.of("2 * 3", 6L),
                Arguments.of("2 * 5", 10L),
                Arguments.of("0 - 0", 0L),
                Arguments.of("2 - 1", 1L),
                Arguments.of("2 - 5", -3L),
                Arguments.of("10 / 5", 2L),
                Arguments.of("2 / 1", 2L),
                Arguments.of("1 / 1", 1L),
                Arguments.of("0 / 5", 0L)
        );
    }

    @ParameterizedTest
    @MethodSource("provideMathExpressions")
    @DisplayName("수식이 맞는 지 판별한다.")
    void expressionCheckerTest(String expression, String expected) {
        ExpressionChecker sut = new ExpressionChecker(expression);

        String actual = sut.check();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideMathExpressions() {
        return Stream.of(
                Arguments.of("3 * 2 = 6", "correct"),
                Arguments.of("1 + 2 = 6", "wrong answer"),
                Arguments.of("0 + 0 = 0", "correct"),
                Arguments.of("3 * 2 = 6", "correct"),
                Arguments.of("11 + 11 = 11", "wrong answer"),
                Arguments.of("7 - 9 = -2", "correct"),
                Arguments.of("3 * 0 = 0", "correct")
        );
    }
}

class ExpressionChecker {

    public static final String DELIMITER = " = ";
    public static final int EXPRESSION_INDEX = 0;
    public static final int ANSWER_INDEX = 1;
    public static final String CORRECT_WORD = "correct";
    public static final String INCORRECT_WORD = "wrong answer";
    private final Expression expression;
    private final long answer;

    public ExpressionChecker(String expression) {
        String[] splitExpression = expression.split(DELIMITER);
        this.expression = new Expression(splitExpression[EXPRESSION_INDEX]);
        this.answer = Long.parseLong(splitExpression[ANSWER_INDEX]);
    }

    public String check() {
        return (expression.calculate() == answer) ? CORRECT_WORD : INCORRECT_WORD;
    }
}

enum Sign {
    PLUS("+", (firstNumber, secondNumber) -> firstNumber + secondNumber),
    MULTIPLY("*", (firstNumber, secondNumber) -> firstNumber * secondNumber),
    MINUS("-", (firstNumber, secondNumber) -> firstNumber - secondNumber),
    DIVIDE("/", (firstNumber, secondNumber) -> firstNumber / secondNumber);

    private final String sign;
    private final ToLongBiFunction<Long, Long> formula;

    Sign(String sign, ToLongBiFunction<Long, Long> formula) {
        this.sign = sign;
        this.formula = formula;
    }

    public static Sign valueOfSign(String signString) {
        return Arrays.stream(values())
                .filter(s -> s.sign.equals(signString))
                .findFirst()
                .orElseThrow();
    }

    public Long apply(long firstNumber, long secondNumber) {
        return formula.applyAsLong(firstNumber, secondNumber);
    }
}

class Expression {

    public static final String DELIMITER = " ";
    public static final int FIRST_NUMBER_INDEX = 0;
    public static final int SECOND_NUMBER_INDEX = 2;
    public static final int SIGN_INDEX = 1;
    private final long firstNumber;
    private final long secondNumber;
    private final Sign sign;

    public Expression(String expression) {
        String[] splitExpression = expression.split(DELIMITER);
        this.firstNumber = Long.parseLong(splitExpression[FIRST_NUMBER_INDEX]);
        this.secondNumber = Long.parseLong(splitExpression[SECOND_NUMBER_INDEX]);
        this.sign = Sign.valueOfSign(splitExpression[SIGN_INDEX]);
    }

    public long calculate() {
        return sign.apply(firstNumber, secondNumber);
    }
}
~~~
