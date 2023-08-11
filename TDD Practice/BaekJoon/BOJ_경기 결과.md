# 경기 결과 (Bronze 3)

### 문제 설명

A와 B가 게임을 한다. 게임은 N번의 라운드로 이루어져 있다. 각 라운드에서는, 더 많은 점수를 얻은 사람이 그 라운드의 승자가 된다. 즉, A의 점수가 B의 점수보다 크면 i번째 라운드는 A의 승리이며, B의 점수가 A의 점수보다 크면 i번째 라운드는 B의 승리이다. 무승부인 경우에는 아무도 승리하지 않는다.

N번의 라운드에서의 A와 B의 점수가 주어졌을 때, A가 이긴 횟수와, B가 이긴 횟수를 출력하는 프로그램을 만들어라.

출처 : https://www.acmicpc.net/problem/5523

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());

        GameResults gameResults = new GameResults(inputGameResults(br, n));
        System.out.println(gameResults.getCountOfWinning());
    }

    private static List<String> inputGameResults(BufferedReader br, int n) throws IOException {
        List<String> resultOfGame = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            resultOfGame.add(br.readLine());
        }
        return resultOfGame;
    }
}

class GameResults {

    public static final String DELIMITER = " ";

    private final List<String> gameResults;

    public GameResults(List<String> gameResults) {
        this.gameResults = gameResults;
    }

    public String getCountOfWinning() {
        return getCountOfWinning(Winner.FIRST) + DELIMITER + getCountOfWinning(Winner.SECOND);
    }

    private int getCountOfWinning(Winner player) {
        return (int) gameResults.stream()
                .map(GameResult::from)
                .filter(result -> result.whoIsWinner().equals(player))
                .count();
    }
}

class GameResult {

    public static final String DELIMITER = " ";
    public static final int FIRST_PLAYER_SCORE_INDEX = 0;
    public static final int SECOND_PLAYER_SCORE_INDEX = 1;

    private final int firstPlayerScore;
    private final int secondPlayerScore;

    private GameResult(int firstPlayerScore, int secondPlayerScore) {
        this.firstPlayerScore = firstPlayerScore;
        this.secondPlayerScore = secondPlayerScore;
    }

    public static GameResult from(String gameResult) {
        String[] splitString = gameResult.split(DELIMITER);
        return new GameResult(Integer.parseInt(splitString[FIRST_PLAYER_SCORE_INDEX]),
                Integer.parseInt(splitString[SECOND_PLAYER_SCORE_INDEX]));
    }

    public Winner whoIsWinner() {
        if (firstPlayerScore == secondPlayerScore) {
            return Winner.DRAW;
        }
        return (firstPlayerScore > secondPlayerScore) ? Winner.FIRST : Winner.SECOND;
    }
}

enum Winner {
    FIRST, SECOND, DRAW;
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
    @MethodSource("provideGameResults")
    @DisplayName("두 명의 경기 결과가 여러개 주어졌을 때 각자 이긴 횟수를 반환한다.")
    void testGetCountOfWinning(List<String> gameResults, String expected) {
        GameResults sut = new GameResults(gameResults);

        String actual = sut.getCountOfWinning();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideGameResults() {
        return Stream.of(
                Arguments.of(List.of("1 1"), "0 0"),
                Arguments.of(List.of("1 0"), "1 0"),
                Arguments.of(List.of("1 0", "0 1"), "1 1"),
                Arguments.of(List.of("1 0", "0 1", "0 0"), "1 1"),
                Arguments.of(List.of("100 0", "5 6", "40 50", "74 75"), "1 3"),
                Arguments.of(List.of("20 20", "3 95", "60 59", "40 40", "20 19"), "2 1")
        );
    }
}

class GameResults {

    public static final String DELIMITER = " ";

    private final List<String> gameResults;

    public GameResults(List<String> gameResults) {
        this.gameResults = gameResults;
    }

    public String getCountOfWinning() {
        return getCountOfWinning(Winner.FIRST) + DELIMITER + getCountOfWinning(Winner.SECOND);
    }

    private int getCountOfWinning(Winner player) {
        return (int) gameResults.stream()
                .map(GameResult::from)
                .filter(result -> result.whoIsWinner().equals(player))
                .count();
    }
}

class GameResult {

    public static final String DELIMITER = " ";
    public static final int FIRST_PLAYER_SCORE_INDEX = 0;
    public static final int SECOND_PLAYER_SCORE_INDEX = 1;

    private final int firstPlayerScore;
    private final int secondPlayerScore;

    private GameResult(int firstPlayerScore, int secondPlayerScore) {
        this.firstPlayerScore = firstPlayerScore;
        this.secondPlayerScore = secondPlayerScore;
    }

    public static GameResult from(String gameResult) {
        String[] splitString = gameResult.split(DELIMITER);
        return new GameResult(Integer.parseInt(splitString[FIRST_PLAYER_SCORE_INDEX]),
                Integer.parseInt(splitString[SECOND_PLAYER_SCORE_INDEX]));
    }

    public Winner whoIsWinner() {
        if (firstPlayerScore == secondPlayerScore) {
            return Winner.DRAW;
        }
        return (firstPlayerScore > secondPlayerScore) ? Winner.FIRST : Winner.SECOND;
    }
}

enum Winner {
    FIRST, SECOND, DRAW;
}
~~~
