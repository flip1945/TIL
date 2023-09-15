# 그래서 님 푼 문제 수가? (Bronze 3)

### 문제 설명

브실이는 하루라도 빨리 대회 출제 자격을 얻기 위해서 $1\,000$문제 해결을 목표로 매일 문제를 풀고 있다. 그러다 보니 다른 사람들의 푼 문제 수에 관심이 많다. 사람들은 “저는 총 $1\,000$문제 이상 해결하려면 하루에 $5$문제씩 최소 $128$일은 더 풀어야 해요”와 같이 자신이 몇 문제를 풀었는지 설명한다. 브실이는 이 말을 들을 때마다 상대방이 현재까지 몇 문제를 풀었는지 궁금해서 참을 수 없었다.

브실이를 도와 상대방이 푼 문제 수의 최솟값과 최댓값을 구해보자.

출처 : https://www.acmicpc.net/problem/17350

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ProblemSolver problemSolver = new ProblemSolver(scanner.nextInt(), scanner.nextInt(), scanner.nextInt());
        System.out.print(problemSolver.countOfProblemSolved());
    }
}

class ProblemSolver {

    private static final String DELIMITER = " ";

    private final int countOfGoal;
    private final int problemSolvedCountPerDay;
    private final int dayOfNeeded;

    public ProblemSolver(int countOfGoal, int problemSolvedCountPerDay, int dayOfNeeded) {
        this.countOfGoal = countOfGoal;
        this.problemSolvedCountPerDay = problemSolvedCountPerDay;
        this.dayOfNeeded = dayOfNeeded;
    }

    public String countOfProblemSolved() {
        return getMinCountOfProblemSolved(getCountOfProblemSolved()) + DELIMITER
                + getMaxCountOfProblemSolved(getCountOfProblemSolved());
    }

    private int getCountOfProblemSolved() {
        return countOfGoal - (problemSolvedCountPerDay * dayOfNeeded);
    }

    private int getMinCountOfProblemSolved(int countOfProblemSolved) {
        return Math.max(countOfProblemSolved, 0);
    }

    private int getMaxCountOfProblemSolved(int countOfProblemSolved) {
        return countOfProblemSolved + problemSolvedCountPerDay - 1;
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
    @MethodSource("provideProblemSolvedInfo")
    @DisplayName("문제를 해결한 수를 계산해야 한다.")
    void ProblemSolverCountProblemSolved(int countOfGoal, int problemSolvedCountPerDay, int dayOfNeeded, String expected) {
        ProblemSolver sut = new ProblemSolver(countOfGoal, problemSolvedCountPerDay, dayOfNeeded);

        String actual = sut.countOfProblemSolved();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideProblemSolvedInfo() {
        return Stream.of(
                Arguments.of(10, 1, 6, "4 4"),
                Arguments.of(10, 1, 8, "2 2"),
                Arguments.of(10, 2, 3, "4 5"),
                Arguments.of(1000, 5, 128, "360 364"),
                Arguments.of(10, 9, 2, "0 0"),
                Arguments.of(10, 5, 2, "0 4"),
                Arguments.of(10, 2000, 1, "0 9")
        );
    }
}

class ProblemSolver {

    private static final String DELIMITER = " ";

    private final int countOfGoal;
    private final int problemSolvedCountPerDay;
    private final int dayOfNeeded;

    public ProblemSolver(int countOfGoal, int problemSolvedCountPerDay, int dayOfNeeded) {
        this.countOfGoal = countOfGoal;
        this.problemSolvedCountPerDay = problemSolvedCountPerDay;
        this.dayOfNeeded = dayOfNeeded;
    }

    public String countOfProblemSolved() {
        return getMinCountOfProblemSolved(getCountOfProblemSolved()) + DELIMITER
                + getMaxCountOfProblemSolved(getCountOfProblemSolved());
    }

    private int getCountOfProblemSolved() {
        return countOfGoal - (problemSolvedCountPerDay * dayOfNeeded);
    }

    private int getMinCountOfProblemSolved(int countOfProblemSolved) {
        return Math.max(countOfProblemSolved, 0);
    }

    private int getMaxCountOfProblemSolved(int countOfProblemSolved) {
        return countOfProblemSolved + problemSolvedCountPerDay - 1;
    }
}
~~~
