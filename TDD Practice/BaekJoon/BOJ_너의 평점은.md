# 너의 평점은 (Silver 5)

### 문제 설명

인하대학교 컴퓨터공학과를 졸업하기 위해서는, 전공평점이 3.3 이상이거나 졸업고사를 통과해야 한다. 그런데 아뿔싸, 치훈이는 깜빡하고 졸업고사를 응시하지 않았다는 사실을 깨달았다!

치훈이의 전공평점을 계산해주는 프로그램을 작성해보자.

전공평점은 전공과목별 (학점 × 과목평점)의 합을 학점의 총합으로 나눈 값이다.

인하대학교 컴퓨터공학과의 등급에 따른 과목평점은 다음 표와 같다.

|A+|	4.5|
|-|-|
|A0|	4.0|
|B+|	3.5|
|B0|	3.0|
|C+|	2.5|
|C0|	2.0|
|D+|	1.5|
|D0|	1.0|
|F|	0.0|

P/F 과목의 경우 등급이 P또는 F로 표시되는데, 등급이 P인 과목은 계산에서 제외해야 한다.

과연 치훈이는 무사히 졸업할 수 있을까?

출처 : https://www.acmicpc.net/problem/25206

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        List<String> testResultLines = new ArrayList<>();
        for (int i = 0; i < 20; i++) {
            testResultLines.add(scanner.nextLine());
        }

        System.out.println(calculateGrade(testResultLines));
    }

    static double calculateGrade(List<String> testResultLines) {
        List<TestResult> testResults = convertToTestResult(testResultLines);
        double sumOfGrade = getSumOfGrade(testResults);
        double totalCredit = getTotalCredit(testResults);
        return sumOfGrade / totalCredit;
    }

    static List<TestResult> convertToTestResult(List<String> testResultLines) {
        return testResultLines.stream()
                .map(TestResult::new)
                .collect(Collectors.toList());
    }

    static double getSumOfGrade(List<TestResult> testResults) {
        double sumOfGrade = 0;
        for (TestResult testResult : testResults) {
            if (testResult.grade == Grade.P) {
                continue;
            }
            sumOfGrade += testResult.grade.getValue() * testResult.credit;
        }
        return sumOfGrade;
    }

    static double getTotalCredit(List<TestResult> testResults) {
        double totalCredit = 0;
        for (TestResult testResult : testResults) {
            if (testResult.grade == Grade.P) {
                continue;
            }
            totalCredit += testResult.credit;
        }
        return totalCredit;
    }
}

enum Grade {
    A_PLUS("A+", 4.5),
    A_ZERO("A0", 4.0),
    B_PLUS("B+", 3.5),
    B_ZERO("B0", 3.0),
    C_PLUS("C+", 2.5),
    C_ZERO("C0", 2.0),
    D_PLUS("D+", 1.5),
    D_ZERO("D0", 1.0),
    F("F", 0),
    P("P", 0);

    private static final Map<String, Grade> gradeMap = new HashMap<>();

    static {
        for (Grade grade : values()) {
            gradeMap.put(grade.gradeString, grade);
        }
    }

    private final String gradeString;
    private final double value;

    public static Grade valueOfGrade(String name) {
        return gradeMap.get(name);
    }

    Grade(String gradeString, double value) {
        this.gradeString = gradeString;
        this.value = value;
    }

    public double getValue() {
        return value;
    }
}

class TestResult {
    double credit;
    Grade grade;

    public TestResult(String testResultLine) {
        String[] testResult = testResultLine.split(" ");
        this.credit = Double.parseDouble(testResult[1]);
        this.grade = Grade.valueOfGrade(testResult[2]);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void calculateGradeTest() {
        assertEquals(4.0, calculateGrade(List.of("A 4.0 A0", "AB 4.0 A0", "ABC 4.0 A0")));
        assertEquals(3.0, calculateGrade(List.of("A 4.0 A0", "AB 4.0 B0", "ABC 4.0 C0")));
        assertEquals(2.477272727272727, calculateGrade(List.of("A 4.0 A+", "AB 4.0 B+", "ABC 4.0 C+", "ABCD 4.0 C0", "ABCDE 3.0 D+", "F 3.0 F")));
        assertEquals(1.0, calculateGrade(List.of("A 4.0 D0", "AB 4.0 P")));
    }

    private double calculateGrade(List<String> testResultLines) {
        List<TestResult> testResults = convertToTestResult(testResultLines);
        double sumOfGrade = getSumOfGrade(testResults);
        double totalCredit = getTotalCredit(testResults);
        return sumOfGrade / totalCredit;
    }

    private List<TestResult> convertToTestResult(List<String> testResultLines) {
        return testResultLines.stream()
                .map(TestResult::new)
                .collect(Collectors.toList());
    }

    private double getSumOfGrade(List<TestResult> testResults) {
        double sumOfGrade = 0;
        for (TestResult testResult : testResults) {
            if (testResult.grade == Grade.P) {
                continue;
            }
            sumOfGrade += testResult.grade.getValue() * testResult.credit;
        }
        return sumOfGrade;
    }

    private double getTotalCredit(List<TestResult> testResults) {
        double totalCredit = 0;
        for (TestResult testResult : testResults) {
            if (testResult.grade == Grade.P) {
                continue;
            }
            totalCredit += testResult.credit;
        }
        return totalCredit;
    }
}

enum Grade {
    A_PLUS("A+", 4.5),
    A_ZERO("A0", 4.0),
    B_PLUS("B+", 3.5),
    B_ZERO("B0", 3.0),
    C_PLUS("C+", 2.5),
    C_ZERO("C0", 2.0),
    D_PLUS("D+", 1.5),
    D_ZERO("D0", 1.0),
    F("F", 0),
    P("P", 0);

    private static final Map<String, Grade> gradeMap = new HashMap<>();

    static {
        for (Grade grade : values()) {
            gradeMap.put(grade.gradeString, grade);
        }
    }

    private final String gradeString;
    private final double value;

    public static Grade valueOfGrade(String name) {
        return gradeMap.get(name);
    }

    Grade(String gradeString, double value) {
        this.gradeString = gradeString;
        this.value = value;
    }

    public double getValue() {
        return value;
    }
}

class TestResult {
    double credit;
    Grade grade;

    public TestResult(String testResultLine) {
        String[] testResult = testResultLine.split(" ");
        this.credit = Double.parseDouble(testResult[1]);
        this.grade = Grade.valueOfGrade(testResult[2]);
    }
}
~~~
