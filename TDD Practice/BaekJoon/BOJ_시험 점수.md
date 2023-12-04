# 시험 점수 (Bronze 4)

### 문제 설명

대한고등학교에 재학 중인 민국이와 만세는 4과목(정보, 수학, 과학, 영어)에 대한 시험을 봤다. 민국이와 만세가 본 4과목의 점수를 입력하면, 민국이의 총점 S와 만세의 총점 T 중에서 큰 점수를 출력하는 프로그램을 작성하시오. 단, 서로 동점일 때는 민국이의 총점 S를 출력한다.

출처 : https://www.acmicpc.net/problem/5596

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Exam firstExam = new Exam(inputExam(scanner));
        Exam secondExam = new Exam(inputExam(scanner));

        Exams exams = new Exams(firstExam, secondExam);
        System.out.println(exams.maxTotalScore());
    }

    private static List<Integer> inputExam(Scanner scanner) {
        return IntStream.range(0, 4)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class Exams {

    private final Exam firstExam;
    private final Exam secondExam;

    public Exams(Exam firstExam, Exam secondExam) {
        this.firstExam = firstExam;
        this.secondExam = secondExam;
    }

    public int maxTotalScore() {
        return Math.max(firstExam.totalScore(), secondExam.totalScore());
    }
}

class Exam {

    private final List<Integer> scores;

    public Exam(List<Integer> scores) {
        this.scores = scores;
    }

    public int totalScore() {
        return scores.stream()
                .mapToInt(Integer::intValue)
                .sum();
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
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideTwoExams")
    @DisplayName("두 명의 시험 점수가 주어지면 더 높은 사람의 총점을 반환한다.")
    void examsReturnMaxTotalScore(Exam firstExam, Exam secondExam, int expected) {
        Exams sut = new Exams(firstExam, secondExam);

        int actual = sut.maxTotalScore();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideTwoExams() {
        return Stream.of(
                Arguments.of(new Exam(List.of(100, 80, 70, 60)), new Exam(List.of(80, 70, 80, 90)), 320),
                Arguments.of(new Exam(List.of(100, 80, 70, 60)), new Exam(List.of(80, 70, 60, 100)), 310)
        );
    }

    @ParameterizedTest
    @MethodSource("provideScores")
    @DisplayName("점수가 주어지면 총점을 반환한다.")
    void examReturnTotalScore(List<Integer> scores, int expected) {
        Exam sut = new Exam(scores);

        int actual = sut.totalScore();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideScores() {
        return Stream.of(
                Arguments.of(List.of(100, 80, 70, 60), 310),
                Arguments.of(List.of(80, 70, 80, 90), 320),
                Arguments.of(List.of(0, 0, 0, 0), 0)
        );
    }
}

class Exams {

    private final Exam firstExam;
    private final Exam secondExam;

    public Exams(Exam firstExam, Exam secondExam) {
        this.firstExam = firstExam;
        this.secondExam = secondExam;
    }

    public int maxTotalScore() {
        return Math.max(firstExam.totalScore(), secondExam.totalScore());
    }
}

class Exam {

    private final List<Integer> scores;

    public Exam(List<Integer> scores) {
        this.scores = scores;
    }

    public int totalScore() {
        return scores.stream()
                .mapToInt(Integer::intValue)
                .sum();
    }
}
~~~
