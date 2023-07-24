# 나의 학점은 (Bronze 2)

### 문제 설명

3학년인 홍익이는 이번 학기 전공필수 과목인 운영체제(OS) 수업을 들었다. 수업을 마치고, 얼마 후 교수님께서 클래스넷을 통해 전 학생의 중간고사, 기말고사, 과제점 점수를 만점 기준 300점으로 환산하여 클래스넷에 올려주셨다. 물끄러미 점수를 바라보던 홍익이는, 불현듯 장학금을 꼭 받아야 된다는 사실이 떠올랐다.

"운영체제 수업을 A0 이상 받지 못하면, 장학금을 받기 어려운데..."

교수님께서는 운영체제 첫 수업 시간에 학점 분포도를 다음과 같이 설명하셨다.

* A+: 1~5등
* A0: 6~15등
* B+: 16~30등
* B0: 31~35등
* C+: 36~45등
* C0: 46~48등
* F: 49~50등

교수님께서는 클래스넷의 수시정보 페이지에 학생들의 최종 점수를 내림차순으로 정렬하여 점수를 보여주셨다.

10일 뒤면 클래스넷을 통해 학점이 공지되지만, 그때까지 참기 어려웠던 홍익이는 프로그램을 통해 자신의 학점을 알아보고자 한다. 홍익이를 도와 홍익이의 학점을 구해보자!

출처 : https://www.acmicpc.net/problem/17826

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Integer> scores = inputScores(scanner);

        Examination examination = new Examination(scores);
        System.out.println(examination.whatIsAGrade(Integer.parseInt(scanner.nextLine())));
    }

    private static List<Integer> inputScores(Scanner scanner) {
        return Arrays.stream(scanner.nextLine().split(" "))
                .map(Integer::parseInt)
                .collect(Collectors.toList());
    }
}

class Examination {

    private final List<Integer> scores;

    public Examination(List<Integer> scores) {
        this.scores = scores;
    }

    public String whatIsAGrade(int examResult) {
        int rank = getRank(examResult);
        Grade grade = Grade.valueOf(rank);
        return grade.getGradeName();
    }

    private int getRank(int examResult) {
        return scores.indexOf(examResult);
    }
}

enum Grade {
    A_PLUS("A+", rank -> rank >= 0 && rank < 5),
    A("A0", rank -> rank >= 5 && rank < 15),
    B_PLUS("B+", rank -> rank >= 15 && rank < 30),
    B("B0", rank -> rank >= 30 && rank < 35),
    C_PLUS("C+", rank -> rank >= 35 && rank < 45),
    C("C0", rank -> rank >= 45 && rank < 48),
    F("F", rank -> false);

    private final String gradeName;
    private final Predicate<Integer> range;

    Grade(String gradeName, Predicate<Integer> range) {
        this.gradeName = gradeName;
        this.range = range;
    }

    public static Grade valueOf(int rank) {
        return Arrays.stream(Grade.values())
                .filter(grade -> grade.range.test(rank))
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

import java.util.*;
import java.util.function.Predicate;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideScores")
    @DisplayName("주어진 점수의 학점을 반환한다.")
    void examinationTest(List<Integer> scores, int examResult, String expected) {
        Examination sut = new Examination(scores);

        String actual = sut.whatIsAGrade(examResult);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideScores() {
        return Stream.of(
                Arguments.of(testScores(), 285, "A+"),
                Arguments.of(testScores(), 251, "A0"),
                Arguments.of(testScores(), 264, "A+"),
                Arguments.of(testScores(), 216, "A0"),
                Arguments.of(testScores(), 205, "B+"),
                Arguments.of(testScores(), 126, "B+"),
                Arguments.of(testScores(), 122, "B0"),
                Arguments.of(testScores(), 80, "B0"),
                Arguments.of(testScores(), 76, "C+"),
                Arguments.of(testScores(), 19, "C+"),
                Arguments.of(testScores(), 14, "C0"),
                Arguments.of(testScores(), 5, "C0"),
                Arguments.of(testScores(), 2, "F"),
                Arguments.of(testScores(), 1, "F")
        );
    }

    private static List<Integer> testScores() {
        return List.of(
                285, 271, 270, 268, 264, 251, 237, 236, 228, 227,
                226, 225, 224, 217, 216, 205, 198, 193, 192, 190,
                182, 168, 165, 161, 157, 146, 141, 135, 127, 126,
                122, 114, 105, 81, 80, 76, 70, 67, 63, 59,
                55, 44, 34, 24, 19, 14, 9, 5, 2, 1
        );
    }
}

class Examination {

    private final List<Integer> scores;

    public Examination(List<Integer> scores) {
        this.scores = scores;
    }

    public String whatIsAGrade(int examResult) {
        int rank = getRank(examResult);
        Grade grade = Grade.valueOf(rank);
        return grade.getGradeName();
    }

    private int getRank(int examResult) {
        return scores.indexOf(examResult);
    }
}

enum Grade {
    A_PLUS("A+", rank -> rank >= 0 && rank < 5),
    A("A0", rank -> rank >= 5 && rank < 15),
    B_PLUS("B+", rank -> rank >= 15 && rank < 30),
    B("B0", rank -> rank >= 30 && rank < 35),
    C_PLUS("C+", rank -> rank >= 35 && rank < 45),
    C("C0", rank -> rank >= 45 && rank < 48),
    F("F", rank -> false);

    private final String gradeName;
    private final Predicate<Integer> range;

    Grade(String gradeName, Predicate<Integer> range) {
        this.gradeName = gradeName;
        this.range = range;
    }

    public static Grade valueOf(int rank) {
        return Arrays.stream(Grade.values())
                .filter(grade -> grade.range.test(rank))
                .findFirst()
                .orElse(F);
    }

    public String getGradeName() {
        return gradeName;
    }
}
~~~
