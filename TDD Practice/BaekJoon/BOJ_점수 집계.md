# 점수 집계 (Bronze 2)

### 문제 설명

한국 체조협회에서는 심판의 오심을 막기 위하여 점수 집계 시스템을 고치기로 하였다. 이전에는 5명의 심판이 1점부터 10점까지 정수의 점수를 주면 최고점과 최저점을 하나씩 제외한 점수의 합을 총점으로 하였다. 이를 보완하기 위해서 최고점과 최저점을 뺀 나머지 3명 점수의 최고점과 최저점의 차이가 4점 이상 나게 되면 점수 조정을 거쳐서 다시 점수를 매기려고 한다. 점수를 집계하여 총점을 계산하거나, 점수 조정을 거쳐서 다시 점수를 매기려고 하는 경우에는 총점 대신 KIN(Keep In Negotiation)을 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/9076

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            Scores scores = new Scores(scanner.nextLine());
            System.out.println(scores.getTotalScore());
        }
    }
}

class Scores {

    public static final String SCORE_DELIMITER = " ";
    public static final String KEEP_IN_NEGOTIATION = "KIN";
    public static final int VALID_FROM_INDEX = 1;
    public static final int VALID_TO_INDEX = 4;
    public static final int KEEP_IN_NEGOTIATION_THRESHOLD = 4;
    private final List<Integer> scores;

    public Scores(String scores) {
        this.scores = getSortedScore(scores);
    }

    private List<Integer> getSortedScore(String scores) {
        return Arrays.stream(scores.split(SCORE_DELIMITER))
                .map(Integer::parseInt)
                .sorted()
                .collect(Collectors.toList());
    }

    public String getTotalScore() {
        return isKeepInNegotiation() ? KEEP_IN_NEGOTIATION : String.valueOf(calculateTotalScore());
    }

    private boolean isKeepInNegotiation() {
        return getDiffBetweenValidHighestScoreAndValidLowestScore() >= KEEP_IN_NEGOTIATION_THRESHOLD;
    }

    private int getDiffBetweenValidHighestScoreAndValidLowestScore() {
        return Math.abs(scores.get(VALID_FROM_INDEX) - scores.get(VALID_TO_INDEX));
    }

    private int calculateTotalScore() {
        return scores.subList(VALID_FROM_INDEX, VALID_TO_INDEX).stream()
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

import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSores")
    @DisplayName("최고점과 최저점을 뺀 나머지 최고점과 최저점의 차이가 4점 이상이면 KIN을 반환하고 아니라면 총점을 반환한다.")
    void calculateScoreTest(String scores, String expected) {
        Scores sut = new Scores(scores);

        String actual = sut.getTotalScore();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSores() {
        return Stream.of(
                Arguments.of("1 2 3 4 5", "9"),
                Arguments.of("2 3 4 5 6", "12"),
                Arguments.of("5 1 4 3 2", "9"),
                Arguments.of("0 100 1 2 6", "KIN"),
                Arguments.of("0 100 1 2 5", "KIN"),
                Arguments.of("10 8 5 7 9", "24"),
                Arguments.of("10 9 10 9 5", "28"),
                Arguments.of("10 3 5 9 10", "KIN"),
                Arguments.of("1 2 3 6 9", "KIN")
        );
    }
}

class Scores {

    public static final String SCORE_DELIMITER = " ";
    public static final String KEEP_IN_NEGOTIATION = "KIN";
    public static final int VALID_FROM_INDEX = 1;
    public static final int VALID_TO_INDEX = 4;
    public static final int KEEP_IN_NEGOTIATION_THRESHOLD = 4;
    private final List<Integer> scores;

    public Scores(String scores) {
        this.scores = getSortedScore(scores);
    }

    private List<Integer> getSortedScore(String scores) {
        return Arrays.stream(scores.split(SCORE_DELIMITER))
                .map(Integer::parseInt)
                .sorted()
                .collect(Collectors.toList());
    }

    public String getTotalScore() {
        return isKeepInNegotiation() ? KEEP_IN_NEGOTIATION : String.valueOf(calculateTotalScore());
    }

    private boolean isKeepInNegotiation() {
        return getDiffBetweenValidHighestScoreAndValidLowestScore() >= KEEP_IN_NEGOTIATION_THRESHOLD;
    }

    private int getDiffBetweenValidHighestScoreAndValidLowestScore() {
        return Math.abs(scores.get(VALID_FROM_INDEX) - scores.get(VALID_TO_INDEX));
    }

    private int calculateTotalScore() {
        return scores.subList(VALID_FROM_INDEX, VALID_TO_INDEX).stream()
                .mapToInt(Integer::intValue)
                .sum();
    }
}
~~~
