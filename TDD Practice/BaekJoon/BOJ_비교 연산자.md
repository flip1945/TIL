# 비교 연산자 (Bronze 2)

### 문제 설명

C언어의 비교 연산자는 아래 표에 나와있다. 

|연산자|	뜻|
|-|-|
|>|	크다|
|>=|	크거나 같다|
|<|	작다|
|<=|	작거나 같다|
|==|	같다|
|!=|	같지 않다|

이 연산자는 두 피연산자를 비교하고, (왼쪽 값과 오른쪽 값) true또는 false (1 또는 0)을 리턴한다. 예를 들어, 2 > 3은 "false"를 리턴하고 (2는 3보다 작기 때문), 3 != 4는 "true", 3 >= 3은 "true"를 리턴한다.

C언어의 비교 연산식이 주어졌을 때, 결과를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/5656

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String expressionString = scanner.nextLine();
        int i = 1;

        while (!expressionString.split(" ")[1].equals("E")) {
            Expression expression = Expression.from(expressionString);
            System.out.println("Case " + i + ": " + expression.executeCompare());

            expressionString = scanner.nextLine();
            i++;
        }
    }
}

class Expression {

    public static final int FIRST_NUMBER_INDEX = 0;
    public static final int SECOND_NUMBER_INDEX = 2;
    public static final int OPERATION_INDEX = 1;
    public static final String DELIMITER = " ";

    private final int firstNumber;
    private final int secondNumber;
    private final Operation operation;

    private Expression(int firstNumber, int secondNumber, Operation operation) {
        this.firstNumber = firstNumber;
        this.secondNumber = secondNumber;
        this.operation = operation;
    }

    public static Expression from(String expression) {
        String[] split = expression.split(DELIMITER);
        return new Expression(Integer.parseInt(split[FIRST_NUMBER_INDEX]),
                Integer.parseInt(split[SECOND_NUMBER_INDEX]), Operation.from(split[OPERATION_INDEX]));
    }

    public boolean executeCompare() {
        return operation.execute(firstNumber, secondNumber);
    }
}

enum Operation {
    EQUALS("==", Objects::equals),
    NOT_EQUALS("!=", (firstNumber, secondNumber) -> !Objects.equals(firstNumber, secondNumber)),
    LESS_THAN("<", (firstNumber, secondNumber) -> firstNumber < secondNumber),
    LESS_THAN_OR_EQUAL_TO("<=", (firstNumber, secondNumber) -> firstNumber <= secondNumber),
    GREATER_THAN(">", (firstNumber, secondNumber) -> firstNumber > secondNumber),
    GREATER_THAN_OR_EQUAL_TO(">=", (firstNumber, secondNumber) -> firstNumber >= secondNumber);

    private final String operation;
    private final BiPredicate<Integer, Integer> function;

    Operation(String operation, BiPredicate<Integer, Integer> function) {
        this.operation = operation;
        this.function = function;
    }

    public static Operation from(String operationString) {
        return Arrays.stream(values())
                .filter(op -> op.operation.equals(operationString))
                .findFirst()
                .orElseThrow();
    }

    public boolean execute(int firstNumber, int secondNumber) {
        return function.test(firstNumber, secondNumber);
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
import java.util.function.BiPredicate;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideExpressions")
    @DisplayName("비교 연산자의 결과를 반한한다.")
    void executeCompareTest(String expression, boolean expected) {
        Expression sut = Expression.from(expression);

        boolean actual = sut.executeCompare();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideExpressions() {
        return Stream.of(
                Arguments.of("1 == 1", true),
                Arguments.of("1 == 2", false),
                Arguments.of("1 == 100", false),
                Arguments.of("1 < 100", true),
                Arguments.of("101 < 100", false),
                Arguments.of("100 < 100", false),
                Arguments.of("100 <= 100", true),
                Arguments.of("101 > 100", true),
                Arguments.of("100 > 100", false),
                Arguments.of("1 > 10", false),
                Arguments.of("10 >= 10", true),
                Arguments.of("9 >= 10", false),
                Arguments.of("12 >= 10", true),
                Arguments.of("12 != 10", true),
                Arguments.of("10 != 10", false)
        );
    }
}

class Expression {

    public static final int FIRST_NUMBER_INDEX = 0;
    public static final int SECOND_NUMBER_INDEX = 2;
    public static final int OPERATION_INDEX = 1;
    public static final String DELIMITER = " ";

    private final int firstNumber;
    private final int secondNumber;
    private final Operation operation;

    private Expression(int firstNumber, int secondNumber, Operation operation) {
        this.firstNumber = firstNumber;
        this.secondNumber = secondNumber;
        this.operation = operation;
    }

    public static Expression from(String expression) {
        String[] split = expression.split(DELIMITER);
        return new Expression(Integer.parseInt(split[FIRST_NUMBER_INDEX]),
                Integer.parseInt(split[SECOND_NUMBER_INDEX]), Operation.from(split[OPERATION_INDEX]));
    }

    public boolean executeCompare() {
        return operation.execute(firstNumber, secondNumber);
    }
}

enum Operation {
    EQUALS("==", Objects::equals),
    NOT_EQUALS("!=", (firstNumber, secondNumber) -> !Objects.equals(firstNumber, secondNumber)),
    LESS_THAN("<", (firstNumber, secondNumber) -> firstNumber < secondNumber),
    LESS_THAN_OR_EQUAL_TO("<=", (firstNumber, secondNumber) -> firstNumber <= secondNumber),
    GREATER_THAN(">", (firstNumber, secondNumber) -> firstNumber > secondNumber),
    GREATER_THAN_OR_EQUAL_TO(">=", (firstNumber, secondNumber) -> firstNumber >= secondNumber);

    private final String operation;
    private final BiPredicate<Integer, Integer> function;

    Operation(String operation, BiPredicate<Integer, Integer> function) {
        this.operation = operation;
        this.function = function;
    }

    public static Operation from(String operationString) {
        return Arrays.stream(values())
                .filter(op -> op.operation.equals(operationString))
                .findFirst()
                .orElseThrow();
    }

    public boolean execute(int firstNumber, int secondNumber) {
        return function.test(firstNumber, secondNumber);
    }
}
~~~
