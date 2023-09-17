# 풀만한문제 (Bronze 2)

### 문제 설명

여름에는 잡초가 많이 자란다.

브실마을에 사는 사람들은 이번 여름 장맛비로 인해 갑자기 훌쩍 커 버린 잡초 때문에 고생하고 있다. 브실마을의 밭에서는 탐스러운 PS 문제들이 자라고 있는데, 잡초에 가려서 문제가 보이지 않게 되기 때문이다.

영어 대소문자와 공백, 숫자로 이루어진 PS 문제의 크기는 각 문제에 포함된 문자의 크기 합으로 정해진다. 대문자의 크기는 $4$, 숫자와 소문자의 크기는 $2$, 공백의 크기는 $1$이다.

이번 브실컵에 내야 하는 문제들을 수확하려던 비행씨의 밭에도 어느새 잡초가 무성하게 자라 버렸다. 코 앞까지 다가온 마감에 골머리를 앓던 비행씨는 잡초를 뽑기 전에, 먼저 잡초보다 더 크게 자란 문제들을 수확해서 내기로 했다. 수확하지 못한 문제들은 다음 대회를 위해 더 키워질 예정이다.

시간이 없는 비행씨를 위해 잡초보다 크기가 크지 않은, 풀만한문제들의 수를 알려 주는 프로그램을 만들어 주자.

출처 : https://www.acmicpc.net/problem/29716

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer stringTokenizer = new StringTokenizer(scanner.nextLine());
        int sizeOfSolvable = Integer.parseInt(stringTokenizer.nextToken());
        int sizeOfProblems = Integer.parseInt(stringTokenizer.nextToken());

        Problems problems = Problems.of(inputProblems(scanner, sizeOfProblems), sizeOfSolvable);
        System.out.println(problems.getCountOfSolvable());
    }

    private static List<String> inputProblems(Scanner scanner, int sizeOfProblems) {
        return IntStream.range(0, sizeOfProblems).mapToObj(i -> scanner.nextLine()).collect(Collectors.toList());
    }
}

class Problems {

    private final List<Problem> problems;
    private final int sizeOfSolvable;

    private Problems(List<Problem> problems, int sizeOfSolvable) {
        this.problems = problems;
        this.sizeOfSolvable = sizeOfSolvable;
    }

    public static Problems of(List<String> problems, int sizeOfSolvable) {
        return new Problems(initProblems(problems), sizeOfSolvable);
    }

    private static List<Problem> initProblems(List<String> problems) {
        return problems.stream()
                .map(Problem::new)
                .collect(Collectors.toList());
    }

    public int getCountOfSolvable() {
        return (int) problems.stream()
                .filter(this::canSolve)
                .count();
    }

    private boolean canSolve(Problem problem) {
        return problem.getSize() <= sizeOfSolvable;
    }
}

class Problem {

    private static final int BIG_SIZE = 4;
    private static final int MIDDLE_SIZE = 2;
    private static final int SMALL_SIZE = 1;

    private final String problem;

    public Problem(String problem) {
        this.problem = problem;
    }

    public int getSize() {
        return problem.chars()
                .map(this::calculateSizeOfProblem)
                .sum();
    }

    private int calculateSizeOfProblem(int problemCharacter) {
        if (Character.isWhitespace(problemCharacter)) {
            return SMALL_SIZE;
        }
        return Character.isUpperCase(problemCharacter) ? BIG_SIZE : MIDDLE_SIZE;
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

import java.util.List;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideProblem")
    @DisplayName("문제의 크기를 계산한다.")
    void problemCalculatesSizeOfProblem(String problem, int expected) {
        Problem sut = new Problem(problem);

        int actual = sut.getSize();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideProblem() {
        return Stream.of(
                Arguments.of("A", 4),
                Arguments.of("a", 2),
                Arguments.of("1", 2),
                Arguments.of(" ", 1),
                Arguments.of("ABC", 12),
                Arguments.of("Ab ", 7),
                Arguments.of("Quatro cheese pizza", 38),
                Arguments.of("Alloy coin", 21),
                Arguments.of("PiPi", 12),
                Arguments.of("1 2 3", 8)
        );
    }

    @ParameterizedTest
    @MethodSource("provideProblems")
    @DisplayName("해결 가능한 문제의 수를 계산한다.")
    void problemsCalculateCountOfSolvableProblems(List<String> problems, int sizeOfSolvable, int expected) {
        Problems sut = Problems.of(problems, sizeOfSolvable);

        int actual = sut.getCountOfSolvable();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideProblems() {
        return Stream.of(
                Arguments.of(List.of(" "), 1, 1),
                Arguments.of(List.of(" ", "A"), 4, 2),
                Arguments.of(List.of(" ", "a", "A"), 4, 3),
                Arguments.of(List.of(" ", "a", "A"), 2, 2),
                Arguments.of(List.of(" ", "a", "A"), 1, 1),
                Arguments.of(List.of("Quatro cheese pizza", "Alloy coin", "PiPi"), 8, 0),
                Arguments.of(List.of("Quatro cheese pizza", "Alloy coin", "PiPi", "1 2 3"), 8, 1)
        );
    }
}

class Problems {

    private final List<Problem> problems;
    private final int sizeOfSolvable;

    private Problems(List<Problem> problems, int sizeOfSolvable) {
        this.problems = problems;
        this.sizeOfSolvable = sizeOfSolvable;
    }

    public static Problems of(List<String> problems, int sizeOfSolvable) {
        return new Problems(initProblems(problems), sizeOfSolvable);
    }

    private static List<Problem> initProblems(List<String> problems) {
        return problems.stream()
                .map(Problem::new)
                .collect(Collectors.toList());
    }

    public int getCountOfSolvable() {
        return (int) problems.stream()
                .filter(this::canSolve)
                .count();
    }

    private boolean canSolve(Problem problem) {
        return problem.getSize() <= sizeOfSolvable;
    }
}

class Problem {

    private static final int BIG_SIZE = 4;
    private static final int MIDDLE_SIZE = 2;
    private static final int SMALL_SIZE = 1;

    private final String problem;

    public Problem(String problem) {
        this.problem = problem;
    }

    public int getSize() {
        return problem.chars()
                .map(this::calculateSizeOfProblem)
                .sum();
    }

    private int calculateSizeOfProblem(int problemCharacter) {
        if (Character.isWhitespace(problemCharacter)) {
            return SMALL_SIZE;
        }
        return Character.isUpperCase(problemCharacter) ? BIG_SIZE : MIDDLE_SIZE;
    }
}
~~~
