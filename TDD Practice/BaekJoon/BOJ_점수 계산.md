# 점수 계산 (Silver 5)

### 문제 설명

상근이는 퀴즈쇼의 PD이다. 이 퀴즈쇼의 참가자는 총 8개 문제를 푼다. 참가자는 각 문제를 풀고, 그 문제를 풀었을 때 얻는 점수는 문제를 풀기 시작한 시간부터 경과한 시간과 난이도로 결정한다. 문제를 풀지 못한 경우에는 0점을 받는다. 참가자의 총 점수는 가장 높은 점수 5개의 합이다. 

상근이는 잠시 여자친구와 전화 통화를 하느라 참가자의 점수를 계산하지 않고 있었다. 참가자의 8개 문제 점수가 주어졌을 때, 총 점수를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/2822

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Integer> quizScores = new ArrayList<>();
        for (int i = 0; i < 8; i++) {
            quizScores.add(scanner.nextInt());
        }

        Scores scores = new Scores(quizScores);
        System.out.println(scores.getValidTotalScore());
        System.out.println(scores.getValidQuizNumbers());
    }
}

class Scores {

    public static final int VALID_TOP_SCORE_INDEX = 5;
    public static final String DELIMITER = " ";
    private final List<Score> validSortedScores;

    public Scores(List<Integer> scores) {
        this.validSortedScores = getValidSortedScores(scores);
    }

    private List<Score> getValidSortedScores(List<Integer> scores) {
        int scoresSize = scores.size();
        return getSortedScore(scores).subList(scoresSize - VALID_TOP_SCORE_INDEX, scoresSize);
    }

    private List<Score> getSortedScore(List<Integer> scores) {
        return IntStream.range(0, scores.size())
                .mapToObj(i -> new Score(scores.get(i), i))
                .sorted()
                .collect(Collectors.toList());
    }

    public int getValidTotalScore() {
        return validSortedScores.stream()
                .mapToInt(Score::getScore)
                .sum();
    }

    public String getValidQuizNumbers() {
        return String.join(DELIMITER, getValidSortedQuizNumbers());
    }

    private List<String> getValidSortedQuizNumbers() {
        return validSortedScores.stream()
                .map(Score::getQuizNumber)
                .sorted()
                .map(String::valueOf)
                .collect(Collectors.toList());
    }
}

class Score implements Comparable<Score> {

    public static final int START_QUIZ_NUMBER = 1;
    private final int score;
    private final int quizNumber;

    public Score(int score, int quizNumber) {
        this.score = score;
        this.quizNumber = quizNumber + START_QUIZ_NUMBER;
    }

    public int getScore() {
        return score;
    }

    public int getQuizNumber() {
        return quizNumber;
    }

    @Override
    public int compareTo(Score o) {
        return this.score - o.score;
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
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideScores")
    @DisplayName("주어진 점수 중 유효한 점수의 총점을 반환한다.")
    void calculateTotalScoreTest(List<Integer> scores, int expected) {
        Scores sut = new Scores(scores);

        int actual = sut.getValidTotalScore();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideScores() {
        return Stream.of(
                Arguments.of(List.of(1, 2, 3, 4, 5, 6, 7, 8), 30),
                Arguments.of(List.of(10, 20, 30, 40, 50, 60, 70, 80), 300),
                Arguments.of(List.of(20, 30, 50, 48, 33, 66, 0, 64), 261),
                Arguments.of(List.of(20, 30, 50, 80, 110, 11, 0, 85), 355),
                Arguments.of(List.of(20, 0, 50, 80, 77, 110, 56, 48), 373)
        );
    }

    @ParameterizedTest
    @MethodSource("provideScoresAndQuizNumber")
    @DisplayName("주어진 점수 중 유효한 점수의 문제 번호를 정렬해서 반환한다.")
    void getValidQuizNumberTest(List<Integer> scores, String expected) {
        Scores sut = new Scores(scores);

        String actual = sut.getValidQuizNumbers();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideScoresAndQuizNumber() {
        return Stream.of(
                Arguments.of(List.of(1, 2, 3, 4, 5, 6, 7, 8), "4 5 6 7 8"),
                Arguments.of(List.of(8, 7, 6, 5, 4, 3, 2, 1), "1 2 3 4 5"),
                Arguments.of(List.of(10, 20, 30, 40, 50, 60, 70, 80), "4 5 6 7 8"),
                Arguments.of(List.of(20, 30, 50, 48, 33, 66, 0, 64), "3 4 5 6 8"),
                Arguments.of(List.of(20, 30, 50, 80, 110, 11, 0, 85), "2 3 4 5 8"),
                Arguments.of(List.of(20, 0, 50, 80, 77, 110, 56, 48), "3 4 5 6 7")
        );
    }
}

class Scores {

    public static final int VALID_TOP_SCORE_INDEX = 5;
    public static final String DELIMITER = " ";
    private final List<Score> validSortedScores;

    public Scores(List<Integer> scores) {
        this.validSortedScores = getValidSortedScores(scores);
    }

    private List<Score> getValidSortedScores(List<Integer> scores) {
        int scoresSize = scores.size();
        return getSortedScore(scores).subList(scoresSize - VALID_TOP_SCORE_INDEX, scoresSize);
    }

    private List<Score> getSortedScore(List<Integer> scores) {
        return IntStream.range(0, scores.size())
                .mapToObj(i -> new Score(scores.get(i), i))
                .sorted()
                .collect(Collectors.toList());
    }

    public int getValidTotalScore() {
        return validSortedScores.stream()
                .mapToInt(Score::getScore)
                .sum();
    }

    public String getValidQuizNumbers() {
        return String.join(DELIMITER, getValidSortedQuizNumbers());
    }

    private List<String> getValidSortedQuizNumbers() {
        return validSortedScores.stream()
                .map(Score::getQuizNumber)
                .sorted()
                .map(String::valueOf)
                .collect(Collectors.toList());
    }
}

class Score implements Comparable<Score> {

    public static final int START_QUIZ_NUMBER = 1;
    private final int score;
    private final int quizNumber;

    public Score(int score, int quizNumber) {
        this.score = score;
        this.quizNumber = quizNumber + START_QUIZ_NUMBER;
    }

    public int getScore() {
        return score;
    }

    public int getQuizNumber() {
        return quizNumber;
    }

    @Override
    public int compareTo(Score o) {
        return this.score - o.score;
    }
}
~~~
