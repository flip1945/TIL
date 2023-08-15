# 내 학점을 구해줘 (Bronze 3)

### 문제 설명

게으른 근우는 열심히 놀다가 문득, 자신의 학점 평균이 얼마일지 궁금해졌다. 학사시스템도 들어가기 귀찮아하는 근우를 위해 구해주도록 하자. 

출처 : https://www.acmicpc.net/problem/10984

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < t; i++) {
            int n = Integer.parseInt(scanner.nextLine());

            GradePointAverage gpa = GradePointAverage.from(inputReportCards(scanner, n));
            System.out.println(gpa.getGradePointAverage());
        }
    }

    private static List<String> inputReportCards(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class GradePointAverage {

    public static final String CREDIT_AND_GPA_FORMAT = "%d %.1f";

    private final List<Grade> grades;

    private GradePointAverage(List<Grade> grades) {
        this.grades = grades;
    }

    public static GradePointAverage from(List<String> reportCards) {
        return new GradePointAverage(initGrades(reportCards));
    }

    private static List<Grade> initGrades(List<String> reportCards) {
        return reportCards.stream()
                .map(Grade::from)
                .collect(Collectors.toList());
    }

    public String getGradePointAverage() {
        return String.format(CREDIT_AND_GPA_FORMAT, getTotalCredit(), calculateGradePointAverage());
    }

    private double calculateGradePointAverage() {
        return getTotalGradePoint() / getTotalCredit();
    }

    private double getTotalGradePoint() {
        return grades.stream()
                .mapToDouble(Grade::getTotalGradePoint)
                .sum();
    }

    private int getTotalCredit() {
        return grades.stream()
                .mapToInt(Grade::getCredit)
                .sum();
    }
}

class Grade {

    public static final String DELIMITER = " ";
    public static final int CREDIT_INDEX = 0;
    public static final int GRADE_INDEX = 1;

    private final int credit;
    private final double grade;

    private Grade(int credit, double grade) {
        this.credit = credit;
        this.grade = grade;
    }

    public static Grade from(String reportCard) {
        String[] splitString = reportCard.split(DELIMITER);
        return new Grade(Integer.parseInt(splitString[CREDIT_INDEX]), Double.parseDouble(splitString[GRADE_INDEX]));
    }

    public int getCredit() {
        return credit;
    }

    public double getTotalGradePoint() {
        return grade * credit;
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
    @MethodSource("provideReportCards")
    @DisplayName("시험 성적 결과가 주어졌을 때 총 학점과 평점을 반환한다.")
    void testCalculateGradePointAverage(List<String> reportCards, String expected) {
        GradePointAverage sut = GradePointAverage.from(reportCards);

        String actual = sut.getGradePointAverage();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideReportCards() {
        return Stream.of(
                Arguments.of(List.of("1 4.3"), "1 4.3"),
                Arguments.of(List.of("1 4.0"), "1 4.0"),
                Arguments.of(List.of("2 4.0"), "2 4.0"),
                Arguments.of(List.of("1 4.0", "1 4.3"), "2 4.2"),
                Arguments.of(List.of("3 4.3", "2 2.0", "4 0.0", "2 4.0"), "11 2.3"),
                Arguments.of(List.of("4 0.0", "4 0.0", "3 0.0"), "11 0.0"),
                Arguments.of(List.of("3 4.3", "2 2", "4 0.0", "2 4.0"), "11 2.3"),
                Arguments.of(List.of("4 0.0", "4 0", "3 0"), "11 0.0")
        );
    }
}

class GradePointAverage {

    private static final String CREDIT_AND_GPA_FORMAT = "%d %.1f";

    private final List<Grade> grades;

    private GradePointAverage(List<Grade> grades) {
        this.grades = grades;
    }

    public static GradePointAverage from(List<String> reportCards) {
        return new GradePointAverage(initGrades(reportCards));
    }

    private static List<Grade> initGrades(List<String> reportCards) {
        return reportCards.stream()
                .map(Grade::from)
                .collect(Collectors.toList());
    }

    public String getGradePointAverage() {
        return String.format(CREDIT_AND_GPA_FORMAT, getTotalCredit(), calculateGradePointAverage());
    }

    private double calculateGradePointAverage() {
        return getTotalGradePoint() / getTotalCredit();
    }

    private double getTotalGradePoint() {
        return grades.stream()
                .mapToDouble(Grade::getTotalGradePoint)
                .sum();
    }

    private int getTotalCredit() {
        return grades.stream()
                .mapToInt(Grade::getCredit)
                .sum();
    }
}

class Grade {

    private static final String DELIMITER = " ";
    private static final int CREDIT_INDEX = 0;
    private static final int GRADE_INDEX = 1;

    private final int credit;
    private final double grade;

    private Grade(int credit, double grade) {
        this.credit = credit;
        this.grade = grade;
    }

    public static Grade from(String reportCard) {
        String[] splitString = reportCard.split(DELIMITER);
        return new Grade(Integer.parseInt(splitString[CREDIT_INDEX]), Double.parseDouble(splitString[GRADE_INDEX]));
    }

    public int getCredit() {
        return credit;
    }

    public double getTotalGradePoint() {
        return grade * credit;
    }
}
~~~
