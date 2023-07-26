# Report Card Time (Bronze 3)

### 문제 설명

Little hobbitses go to hobbit school in the Shire. They just finished a course, which involved tea-making, meal-eating, nap-taking, and gardening. Based on the following grading scale, assign each hobbit a letter grade based on their final numerical course grade.

* A+: 97-100
* A: 90-96
* B+: 87-89
* B: 80-86
* C+: 77-79
* C: 70-76
* D+: 67-69
* D: 60-66
* F: 0-59

출처 : https://www.acmicpc.net/problem/11367

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            Examination examination = new Examination();
            System.out.println(examination.getGrade(scanner.nextLine()));
        }
    }
}

class Examination {

    public static final String DELIMITER = " ";

    public String getGrade(String nameAndScore) {
        return getExamResult(TestResult.from(nameAndScore));
    }

    private String getExamResult(TestResult testResult) {
        return testResult.getName() + DELIMITER + Grade.valueOf(testResult.getScore()).getGradeName();
    }
}

class TestResult {

    public static final String DELIMITER = " ";
    public static final int NAME_INDEX = 0;
    public static final int SCORE_INDEX = 1;

    private final String name;
    private final int score;

    private TestResult(String name, int score) {
        this.name = name;
        this.score = score;
    }

    public static TestResult from(String nameAndScore) {
        String[] split = nameAndScore.split(DELIMITER);
        return new TestResult(split[NAME_INDEX], Integer.parseInt(split[SCORE_INDEX]));
    }

    public String getName() {
        return name;
    }

    public int getScore() {
        return score;
    }
}

enum Grade {
    A_PLUS("A+", score -> score >= 97),
    A("A", score -> score >= 90 && score < 97),
    B_PLUS("B+", score -> score >= 87 && score < 90),
    B("B", score -> score >= 80 && score < 87),
    C_PLUS("C+", score -> score >= 77 && score < 80),
    C("C", score -> score >= 70 && score < 77),
    D_PLUS("D+", score -> score >= 67 && score < 70),
    D("D", score -> score >= 60 && score < 67),
    F("F", score -> false);

    private final String gradeName;
    private final Predicate<Integer> condition;

    Grade(String gradeName, Predicate<Integer> condition) {
        this.gradeName = gradeName;
        this.condition = condition;
    }

    public static Grade valueOf(int score) {
        return Arrays.stream(values())
                .filter(grade -> grade.condition.test(score))
                .findFirst()
                .orElse(F);
    }

    public String getGradeName() {
        return gradeName;
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
import java.util.function.Predicate;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNamesAndScores")
    @DisplayName("점수가 주어지면 등급을 반환한다.")
    void getGradeTest(String nameAndScore, String expected) {
        Examination sut = new Examination();

        String actual = sut.getGrade(nameAndScore);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNamesAndScores() {
        return Stream.of(
                Arguments.of("a 0", "a F"),
                Arguments.of("b 59", "b F"),
                Arguments.of("c 60", "c D"),
                Arguments.of("d 66", "d D"),
                Arguments.of("e 67", "e D+"),
                Arguments.of("e 69", "e D+"),
                Arguments.of("f 70", "f C"),
                Arguments.of("g 76", "g C"),
                Arguments.of("g 77", "g C+"),
                Arguments.of("g 79", "g C+"),
                Arguments.of("h 80", "h B"),
                Arguments.of("h 86", "h B"),
                Arguments.of("i 87", "i B+"),
                Arguments.of("i 89", "i B+"),
                Arguments.of("j 90", "j A"),
                Arguments.of("ab 96", "ab A"),
                Arguments.of("abc 97", "abc A+"),
                Arguments.of("abcd 100", "abcd A+")
        );
    }
}

class Examination {

    public static final String DELIMITER = " ";

    public String getGrade(String nameAndScore) {
        return getExamResult(TestResult.from(nameAndScore));
    }

    private String getExamResult(TestResult testResult) {
        return testResult.getName() + DELIMITER + Grade.valueOf(testResult.getScore()).getGradeName();
    }
}

class TestResult {

    public static final String DELIMITER = " ";
    public static final int NAME_INDEX = 0;
    public static final int SCORE_INDEX = 1;

    private final String name;
    private final int score;

    private TestResult(String name, int score) {
        this.name = name;
        this.score = score;
    }

    public static TestResult from(String nameAndScore) {
        String[] split = nameAndScore.split(DELIMITER);
        return new TestResult(split[NAME_INDEX], Integer.parseInt(split[SCORE_INDEX]));
    }

    public String getName() {
        return name;
    }

    public int getScore() {
        return score;
    }
}

enum Grade {
    A_PLUS("A+", score -> score >= 97),
    A("A", score -> score >= 90 && score < 97),
    B_PLUS("B+", score -> score >= 87 && score < 90),
    B("B", score -> score >= 80 && score < 87),
    C_PLUS("C+", score -> score >= 77 && score < 80),
    C("C", score -> score >= 70 && score < 77),
    D_PLUS("D+", score -> score >= 67 && score < 70),
    D("D", score -> score >= 60 && score < 67),
    F("F", score -> false);

    private final String gradeName;
    private final Predicate<Integer> condition;

    Grade(String gradeName, Predicate<Integer> condition) {
        this.gradeName = gradeName;
        this.condition = condition;
    }

    public static Grade valueOf(int score) {
        return Arrays.stream(values())
                .filter(grade -> grade.condition.test(score))
                .findFirst()
                .orElse(F);
    }

    public String getGradeName() {
        return gradeName;
    }
}
~~~
