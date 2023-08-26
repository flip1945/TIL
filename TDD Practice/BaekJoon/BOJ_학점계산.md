# 학점계산 (Bronze 5)

### 문제 설명

어떤 사람의 C언어 성적이 주어졌을 때, 평점은 몇 점인지 출력하는 프로그램을 작성하시오.

A+: 4.3, A0: 4.0, A-: 3.7

B+: 3.3, B0: 3.0, B-: 2.7

C+: 2.3, C0: 2.0, C-: 1.7

D+: 1.3, D0: 1.0, D-: 0.7

F: 0.0

출처 : https://www.acmicpc.net/problem/2754

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        Scanner scanner = new Scanner(System.in);
        String grade = scanner.nextLine();

        ReportCard reportCard = ReportCard.from(grade);
        System.out.println(reportCard.getGradePoint());
    }
}

class ReportCard {

    private final Grade grade;

    private ReportCard(Grade grade) {
        this.grade = grade;
    }

    public static ReportCard from(String grade) {
        return new ReportCard(Grade.from(grade));
    }

    public double getGradePoint() {
        return grade.getGradePoint();
    }
}

enum Grade {
    A_PLUS("A+", 4.3),
    A_ZERO("A0", 4.0),
    A_MINUS("A-", 3.7),
    B_PLUS("B+", 3.3),
    B_ZERO("B0", 3.0),
    B_MINUS("B-", 2.7),
    C_PLUS("C+", 2.3),
    C_ZERO("C0", 2.0),
    C_MINUS("C-", 1.7),
    D_PLUS("D+", 1.3),
    D_ZERO("D0", 1.0),
    D_MINUS("D-", 0.7),
    F("F", 0.0);

    private final String grade;
    private final double gradePoint;

    Grade(String grade, double gradePoint) {
        this.grade = grade;
        this.gradePoint = gradePoint;
    }

    public static Grade from(String grade) {
        for (Grade value : values()) {
            if (Objects.equals(value.grade, grade)) {
                return value;
            }
        }
        throw new IllegalArgumentException();
    }

    public double getGradePoint() {
        return gradePoint;
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

import java.util.Objects;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideGrade")
    @DisplayName("학점이 주어지면 평점을 반환한다.")
    void shouldReturnGradePoint(String grade, double expected) {
        ReportCard sut = ReportCard.from(grade);

        double actual = sut.getGradePoint();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideGrade() {
        return Stream.of(
                Arguments.of("A+", 4.3),
                Arguments.of("A0", 4.0),
                Arguments.of("A-", 3.7),
                Arguments.of("B+", 3.3),
                Arguments.of("B0", 3.0),
                Arguments.of("B-", 2.7),
                Arguments.of("C+", 2.3),
                Arguments.of("C0", 2.0),
                Arguments.of("C-", 1.7),
                Arguments.of("D+", 1.3),
                Arguments.of("D0", 1.0),
                Arguments.of("D-", 0.7),
                Arguments.of("F", 0.0)
        );
    }
}

class ReportCard {

    private final Grade grade;

    private ReportCard(Grade grade) {
        this.grade = grade;
    }

    public static ReportCard from(String grade) {
        return new ReportCard(Grade.from(grade));
    }

    public double getGradePoint() {
        return grade.getGradePoint();
    }
}

enum Grade {
    A_PLUS("A+", 4.3),
    A_ZERO("A0", 4.0),
    A_MINUS("A-", 3.7),
    B_PLUS("B+", 3.3),
    B_ZERO("B0", 3.0),
    B_MINUS("B-", 2.7),
    C_PLUS("C+", 2.3),
    C_ZERO("C0", 2.0),
    C_MINUS("C-", 1.7),
    D_PLUS("D+", 1.3),
    D_ZERO("D0", 1.0),
    D_MINUS("D-", 0.7),
    F("F", 0.0);

    private final String grade;
    private final double gradePoint;

    Grade(String grade, double gradePoint) {
        this.grade = grade;
        this.gradePoint = gradePoint;
    }

    public static Grade from(String grade) {
        for (Grade value : values()) {
            if (Objects.equals(value.grade, grade)) {
                return value;
            }
        }
        throw new IllegalArgumentException();
    }

    public double getGradePoint() {
        return gradePoint;
    }
}
~~~
