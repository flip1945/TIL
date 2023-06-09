# 콘테스트 (Bronze 2)

### 문제 설명

최근 온라인에서의 프로그래밍 콘테스트가 열렸다. W 대학과 K 대학의 컴퓨터 클럽은 이전부터 라이벌 관계에있어,이 콘테스트를 이용하여 양자의 우열을 정하자라는 것이되었다.

이번이 두 대학에서 모두 10 명씩이 콘테스트에 참여했다. 긴 논의 끝에 참가한 10 명 중 득점이 높은 사람에서 3 명의 점수를 합산하여 대학의 득점으로하기로 했다.

W 대학 및 K 대학 참가자의 점수 데이터가 주어진다. 이때, 각각의 대학의 점수를 계산하는 프로그램을 작성하라.

출처 : https://www.acmicpc.net/problem/5576

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Integer> scores = inputScores(scanner);

        Contest contest = new Contest(scores);
        printScores(contest);
    }

    private static List<Integer> inputScores(Scanner scanner) {
        return IntStream.range(0, 20)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());
    }

    private static void printScores(Contest contest) {
        contest.getScoresOfTwoUniversity().forEach(score -> System.out.print(score + " "));
    }
}

class Contest {

    public static final int FIRST_UNIVERSITY_FROM_INDEX = 0;
    public static final int FIRST_UNIVERSITY_TO_INDEX = 10;
    public static final int SECOND_UNIVERSITY_FROM_INDEX = 10;
    public static final int SECOND_UNIVERSITY_TO_INDEX = 20;
    private final University firstUniversity;
    private final University secondUniversity;

    public Contest(List<Integer> scores) {
        this.firstUniversity = new University(scores.subList(FIRST_UNIVERSITY_FROM_INDEX, FIRST_UNIVERSITY_TO_INDEX));
        this.secondUniversity = new University(scores.subList(SECOND_UNIVERSITY_FROM_INDEX, SECOND_UNIVERSITY_TO_INDEX));
    }

    public List<Integer> getScoresOfTwoUniversity() {
        return List.of(firstUniversity.getScore(), secondUniversity.getScore());
    }
}

class University {

    private final List<Integer> scores;

    public University(List<Integer> scores) {
        this.scores = scores;
    }

    public int getScore() {
        return scores.stream()
                .sorted(Comparator.reverseOrder())
                .limit(3)
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

import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideScores")
    @DisplayName("대학의 점수를 반환한다.")
    void universityTest(List<Integer> scores, int expected) {
        University sut = new University(scores);
        
        int actual = sut.getScore();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideScores() {
        return Stream.of(
                Arguments.of(List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10), 27),
                Arguments.of(List.of(0, 0, 0, 0, 0, 0, 0, 0, 0, 0), 0),
                Arguments.of(List.of(0, 0, 0, 0, 0, 0, 0, 0, 0, 1), 1),
                Arguments.of(List.of(100, 100, 100, 100, 100, 100, 100, 100, 100, 100), 300),
                Arguments.of(List.of(23, 23, 20, 15, 15, 14, 13, 9, 7, 6), 66),
                Arguments.of(List.of(25, 19, 17, 17, 16, 13, 12, 11, 9, 5), 61)
        );
    }

    @ParameterizedTest
    @MethodSource("provideUniversityScores")
    @DisplayName("각 대학의 점수를 반환한다.")
    void contestTest(List<Integer> scores, List<Integer> expected) {
        Contest sut = new Contest(scores);

        List<Integer> actual = sut.getScoresOfTwoUniversity();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideUniversityScores() {
        return Stream.of(
                Arguments.of(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0), List.of(27, 0)),
                Arguments.of(Arrays.asList(100, 100, 100, 100, 100, 100, 100, 100, 100, 100, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0), List.of(300, 0)),
                Arguments.of(Arrays.asList(23, 23, 20, 15, 15, 14, 13, 9, 7, 6, 25, 19, 17, 17, 16, 13, 12, 11, 9, 5), List.of(66, 61))
        );
    }
}

class Contest {

    public static final int FIRST_UNIVERSITY_FROM_INDEX = 0;
    public static final int FIRST_UNIVERSITY_TO_INDEX = 10;
    public static final int SECOND_UNIVERSITY_FROM_INDEX = 10;
    public static final int SECOND_UNIVERSITY_TO_INDEX = 20;
    private final University firstUniversity;
    private final University secondUniversity;

    public Contest(List<Integer> scores) {
        this.firstUniversity = new University(scores.subList(FIRST_UNIVERSITY_FROM_INDEX, FIRST_UNIVERSITY_TO_INDEX));
        this.secondUniversity = new University(scores.subList(SECOND_UNIVERSITY_FROM_INDEX, SECOND_UNIVERSITY_TO_INDEX));
    }

    public List<Integer> getScoresOfTwoUniversity() {
        return List.of(firstUniversity.getScore(), secondUniversity.getScore());
    }
}

class University {

    private final List<Integer> scores;

    public University(List<Integer> scores) {
        this.scores = scores;
    }

    public int getScore() {
        return scores.stream()
                .sorted(Comparator.reverseOrder())
                .limit(3)
                .mapToInt(Integer::intValue)
                .sum();
    }
}
~~~
