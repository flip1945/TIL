# 나는 요리사다 (Bronze 3)

### 문제 설명

"나는 요리사다"는 다섯 참가자들이 서로의 요리 실력을 뽐내는 티비 프로이다. 각 참가자는 자신있는 음식을 하나씩 만들어오고, 서로 다른 사람의 음식을 점수로 평가해준다. 점수는 1점부터 5점까지 있다.

각 참가자가 얻은 점수는 다른 사람이 평가해 준 점수의 합이다. 이 쇼의 우승자는 가장 많은 점수를 얻은 사람이 된다.

각 참가자가 얻은 평가 점수가 주어졌을 때, 우승자와 그의 점수를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/2953

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<String> contestResult = inputContestResults(scanner);
        CookingContest cookingContest = new CookingContest(contestResult);
        System.out.println(cookingContest.getWinner());
    }

    private static List<String> inputContestResults(Scanner scanner) {
        return IntStream.range(0, 5)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class CookingContest {

    private final List<String> contestResults;

    public CookingContest(List<String> contestResults) {
        this.contestResults = contestResults;
    }

    public String getWinner() {
        return getWinnerScore().toString();
    }

    private Score getWinnerScore() {
        return IntStream.range(0, contestResults.size())
                .mapToObj(i -> new Score(i + 1, contestResults.get(i)))
                .sorted()
                .findFirst()
                .orElseThrow();
    }
}

class Score implements Comparable<Score> {

    public static final String DELIMITER = " ";
    private final int chefId;
    private final int score;

    public Score(int chefId, String scores) {
        this.chefId = chefId;
        this.score = getScore(scores);
    }

    private int getScore(String contestResult) {
        return Arrays.stream(contestResult.split(DELIMITER))
                .mapToInt(Integer::parseInt)
                .sum();
    }

    @Override
    public String toString() {
        return chefId +  " " + score;
    }

    @Override
    public int compareTo(Score o) {
        return Integer.compare(o.score, this.score);
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
import java.util.List;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideContestResults")
    @DisplayName("투표의 당선된 사람의 이름을 반환한다.")
    void cookingContestTest(List<String> contestResults, String expected) {
        CookingContest sut = new CookingContest(contestResults);

        String actual = sut.getWinner();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideContestResults() {
        return Stream.of(
                Arguments.of(List.of("1 1 1 1", "1 1 1 2"), "2 5"),
                Arguments.of(List.of("1 1 1 1", "1 1 1 3"), "2 6"),
                Arguments.of(List.of("1 1 1 4", "1 1 1 3"), "1 7"),
                Arguments.of(List.of("5 4 4 5", "5 4 4 4", "5 5 4 4", "5 5 5 4", "4 4 4 5"), "4 19"),
                Arguments.of(List.of("4 4 3 3", "5 4 3 5", "5 5 2 4", "5 5 5 1", "4 4 4 4"), "2 17")
        );
    }
}

class CookingContest {

    private final List<String> contestResults;

    public CookingContest(List<String> contestResults) {
        this.contestResults = contestResults;
    }

    public String getWinner() {
        return getWinnerScore().toString();
    }

    private Score getWinnerScore() {
        return IntStream.range(0, contestResults.size())
                .mapToObj(i -> new Score(i + 1, contestResults.get(i)))
                .sorted()
                .findFirst()
                .orElseThrow();
    }
}

class Score implements Comparable<Score> {

    public static final String DELIMITER = " ";
    private final int chefId;
    private final int score;

    public Score(int chefId, String scores) {
        this.chefId = chefId;
        this.score = getScore(scores);
    }

    private int getScore(String contestResult) {
        return Arrays.stream(contestResult.split(DELIMITER))
                .mapToInt(Integer::parseInt)
                .sum();
    }

    @Override
    public String toString() {
        return chefId +  " " + score;
    }

    @Override
    public int compareTo(Score o) {
        return Integer.compare(o.score, this.score);
    }
}
~~~
