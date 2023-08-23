# 소음 (Bronze 3)

### 문제 설명

수업 시간에 떠드는 두 학생이 있다. 두 학생은 수업에 집중하는 대신에 글로벌 경제 위기에 대해서 토론하고 있었다. 토론이 점점 과열되면서 두 학생은 목소리를 높였고, 결국 선생님은 크게 분노하였다.

이렇게 학생들이 수업 시간에 떠드는 문제는 어떻게 해결해야 할까?

얼마전에 초등학교 선생님으로 취직한 상근이는 이 문제를 수학 문제로 해결한다. 학생들을 진정시키기 위해 칠판에 수학 문제를 써주고, 아이들에게 조용히 이 문제를 풀게 한다. 학생들이 문제를 금방 풀고 다시 떠드는 것을 방지하기 위해서, 숫자를 매우 크게 한다.

아직 초등학교이기 때문에, 학생들은 덧셈과 곱셈만 배웠다. 또, 아직 10의 제곱꼴을 제외한 다른 수는 학교에서 배우지 않았기 때문에, 선생님이 써주는 수는 모두 10의 제곱 형태이다.

쉬는 시간까지 문제를 푸는 것을 막기 위해서, 선생님이 써주는 숫자는 최대 100자리이다.

칠판에 쓰여 있는 문제가 주어졌을 때, 결과를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/2935

---

#### 풀이
~~~java
import java.math.BigInteger;
import java.util.*;
import java.util.function.BinaryOperator;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Calculator calculator = Calculator.of(scanner.nextLine(), scanner.nextLine(), scanner.nextLine());
        System.out.println(calculator.calculate());
    }
}

class Calculator {

    private final BigInteger firstNumber;
    private final BigInteger secondNumber;
    private final Operation operation;

    private Calculator(BigInteger firstNumber, BigInteger secondNumber, Operation operation) {
        this.firstNumber = firstNumber;
        this.secondNumber = secondNumber;
        this.operation = operation;
    }

    public static Calculator of(String firstNumber, String operation, String secondNumber) {
        return new Calculator(new BigInteger(firstNumber), new BigInteger(secondNumber), Operation.from(operation));
    }

    public String calculate() {
        return operation.execute(firstNumber, secondNumber).toString();
    }
}

enum Operation {
    PlUS("+", BigInteger::add),
    MULTIPLY("*", BigInteger::multiply);

    private final String operation;
    private final BinaryOperator<BigInteger> function;

    Operation(String operation, BinaryOperator<BigInteger> function) {
        this.operation = operation;
        this.function = function;
    }

    public static Operation from(String operation) {
        for (Operation value : values()) {
            if (value.operation.equals(operation)) {
                return value;
            }
        }
        throw new IllegalArgumentException();
    }

    public BigInteger execute(BigInteger firstNumber, BigInteger secondNumber) {
        return function.apply(firstNumber, secondNumber);
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

import java.math.BigInteger;
import java.util.function.BinaryOperator;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideExpression")
    @DisplayName("10제곱 형태의 숫자 2개와 연산자를 입력받아 연산결과를 반환한다.")
    void shouldCalculateExpression(String number1, String operation, String number2, String expected) {
        Calculator sut = Calculator.of(number1, operation, number2);

        String actual = sut.calculate();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideExpression() {
        return Stream.of(
                Arguments.of("10", "*", "10", "100"),
                Arguments.of("10", "*", "100", "1000"),
                Arguments.of("10", "+", "100", "110"),
                Arguments.of("10", "+", "10", "20"),
                Arguments.of("1000", "*", "100", "100000"),
                Arguments.of("10000000000", "*", "10000000000", "100000000000000000000"),
                Arguments.of("100000000000000000000", "+", "100000000000000000000", "200000000000000000000"),
                Arguments.of("10", "+", "1", "11"),
                Arguments.of("1", "+", "10", "11")
        );
    }
}

class Calculator {

    private final BigInteger firstNumber;
    private final BigInteger secondNumber;
    private final Operation operation;

    private Calculator(BigInteger firstNumber, BigInteger secondNumber, Operation operation) {
        this.firstNumber = firstNumber;
        this.secondNumber = secondNumber;
        this.operation = operation;
    }

    public static Calculator of(String firstNumber, String operation, String secondNumber) {
        return new Calculator(new BigInteger(firstNumber), new BigInteger(secondNumber), Operation.from(operation));
    }

    public String calculate() {
        return operation.execute(firstNumber, secondNumber).toString();
    }
}

enum Operation {
    PlUS("+", BigInteger::add),
    MULTIPLY("*", BigInteger::multiply);

    private final String operation;
    private final BinaryOperator<BigInteger> function;

    Operation(String operation, BinaryOperator<BigInteger> function) {
        this.operation = operation;
        this.function = function;
    }

    public static Operation from(String operation) {
        for (Operation value : values()) {
            if (value.operation.equals(operation)) {
                return value;
            }
        }
        throw new IllegalArgumentException();
    }

    public BigInteger execute(BigInteger firstNumber, BigInteger secondNumber) {
        return function.apply(firstNumber, secondNumber);
    }
}
~~~
