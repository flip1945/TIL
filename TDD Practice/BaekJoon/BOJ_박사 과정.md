# 박사 과정 (Bronze 2)

### 문제 설명

동혁이는 박사 학위 논문을 쓰던 중 두 수를 더하는 방법을 까먹었다. 동혁이는 덧셈 문제와 컴퓨터 과학 문제로 이루어진 문제지를 풀어야 군면제를 받을 수 있다.

문제지의 덧셈 문제는 "a+b"와 같은 형식이고, 컴퓨터 과학 문제는 "P=NP" 하나이다. 동혁이의 문제지가 주어졌을 때, 답을 모두 구하는 프로그램을 작성하시오. 

출처 : https://www.acmicpc.net/problem/5026

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        SpecialAdd specialAdd = new SpecialAdd();
        for (int i = 0; i < n; i++) {
            System.out.println(specialAdd.add(scanner.nextLine()));
        }
    }
}

class SpecialAdd {

    private static final String P_NP_PROBLEM = "P=NP";
    private static final String SKIPPED = "skipped";

    public String add(String problem) {
        if (isPEqualsNpProblem(problem)) {
            return SKIPPED;
        }
        return sum(problem);
    }

    private boolean isPEqualsNpProblem(String problem) {
        return P_NP_PROBLEM.equals(problem);
    }

    private String sum(String problem) {
        Expression expression = Expression.from(problem);
        return String.valueOf(expression.getAugend() + expression.getAddend());
    }
}

class Expression {

    private static final String OPERATION = "\\+";
    private static final int AUGEND_INDEX = 0;
    private static final int ADDEND_INDEX = 1;

    private final int augend;
    private final int addend;

    public Expression(int augend, int addend) {
        this.augend = augend;
        this.addend = addend;
    }

    public static Expression from(String expression) {
        String[] split = expression.split(OPERATION);
        return new Expression(Integer.parseInt(split[AUGEND_INDEX]), Integer.parseInt(split[ADDEND_INDEX]));
    }

    public int getAugend() {
        return augend;
    }

    public int getAddend() {
        return addend;
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
    @MethodSource("provideProblem")
    @DisplayName("P=NP가 문제인 경우는 skipped를 덧셈 문제인 경우는 결과를 반환한다.")
    void specialAddReturnsSkippedGivenPEqualsNpProblem(String problem, String expected) {
        SpecialAdd sut = new SpecialAdd();

        String actual = sut.add(problem);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideProblem() {
        return Stream.of(
                Arguments.of("1+1", "2"),
                Arguments.of("1+2", "3"),
                Arguments.of("P=NP", "skipped"),
                Arguments.of("0+0", "0"),
                Arguments.of("1000+1000", "2000")
        );
    }
}

class SpecialAdd {

    private static final String P_NP_PROBLEM = "P=NP";
    private static final String SKIPPED = "skipped";

    public String add(String problem) {
        if (isPEqualsNpProblem(problem)) {
            return SKIPPED;
        }
        return sum(problem);
    }

    private boolean isPEqualsNpProblem(String problem) {
        return P_NP_PROBLEM.equals(problem);
    }

    private String sum(String problem) {
        Expression expression = Expression.from(problem);
        return String.valueOf(expression.getAugend() + expression.getAddend());
    }
}

class Expression {

    private static final String OPERATION = "\\+";
    private static final int AUGEND_INDEX = 0;
    private static final int ADDEND_INDEX = 1;

    private final int augend;
    private final int addend;

    public Expression(int augend, int addend) {
        this.augend = augend;
        this.addend = addend;
    }

    public static Expression from(String expression) {
        String[] split = expression.split(OPERATION);
        return new Expression(Integer.parseInt(split[AUGEND_INDEX]), Integer.parseInt(split[ADDEND_INDEX]));
    }

    public int getAugend() {
        return augend;
    }

    public int getAddend() {
        return addend;
    }
}
~~~
