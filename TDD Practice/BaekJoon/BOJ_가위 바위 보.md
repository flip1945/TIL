# 가위 바위 보 (Bronze 3)

### 문제 설명

가위 바위 보는 두 명이서 하는 게임이다. 보통 미리 정해놓은 수 만큼 게임을 하고, 많은 게임을 이긴 사람이 최종 승자가 된다.

가위 바위 보를 한 횟수와 매번 두 명이 무엇을 냈는지가 주어졌을 때, 최종 승자를 출력하는 프로그램을 작성하시오.

* 바위는 가위를 이긴다.
* 가위는 보를 이긴다.
* 보는 바위를 이긴다.

출처 : https://www.acmicpc.net/problem/4493

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < t; i++) {
            int n = Integer.parseInt(scanner.nextLine());
            Game game = new Game();
            System.out.println(game.getWinner(inputGameResults(scanner, n)));
        }
    }

    private static List<String> inputGameResults(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Game {

    private int firstPlayerScore;
    private int secondPlayerScore;

    public String getWinner(List<String> gameResults) {
        playGame(gameResults);
        return getWinnerOfScores();
    }

    private String getWinnerOfScores() {
        if (firstPlayerScore == secondPlayerScore) {
            return "TIE";
        }
        return firstPlayerScore > secondPlayerScore ? "Player 1" : "Player 2";
    }

    private void playGame(List<String> gameResults) {
        for (String gameResult : gameResults) {
            RockScissorsPaperGame game = RockScissorsPaperGame.from(gameResult);
            plusWinnerScore(game.play());
        }
    }

    private void plusWinnerScore(Winner winner) {
        if (winner.equals(Winner.PLAYER_1)) {
            firstPlayerScore++;
        } else if (winner.equals(Winner.PLAYER_2)) {
            secondPlayerScore++;
        }
    }
}

class RockScissorsPaperGame {

    public static final String DELIMITER = " ";
    public static final int FIRST_PLAYER_SUBMIT_INDEX = 0;
    public static final int SECOND_PLAYER_SUBMIT_INDEX = 1;

    private final RockScissorsPaper firstPlayerSubmit;
    private final RockScissorsPaper secondPlayerSubmit;

    private RockScissorsPaperGame(RockScissorsPaper firstPlayerSubmit, RockScissorsPaper secondPlayerSubmit) {
        this.firstPlayerSubmit = firstPlayerSubmit;
        this.secondPlayerSubmit = secondPlayerSubmit;
    }

    public static RockScissorsPaperGame from(String submit) {
        String[] splitString = submit.split(DELIMITER);
        return new RockScissorsPaperGame(RockScissorsPaper.fromInitial(splitString[FIRST_PLAYER_SUBMIT_INDEX]),
                RockScissorsPaper.fromInitial(splitString[SECOND_PLAYER_SUBMIT_INDEX]));
    }

    public Winner play() {
        return firstPlayerSubmit.getWinner(secondPlayerSubmit);
    }
}

enum RockScissorsPaper {
    ROCK("R", submit -> submit.equals("S") ? Winner.PLAYER_1 : Winner.PLAYER_2),
    SCISSORS("S", submit -> submit.equals("P") ? Winner.PLAYER_1 : Winner.PLAYER_2),
    PAPER("P", submit -> submit.equals("R") ? Winner.PLAYER_1 : Winner.PLAYER_2);

    private final String initial;
    private final Function<String, Winner> playFunction;

    RockScissorsPaper(String initial, Function<String, Winner> playFunction) {
        this.initial = initial;
        this.playFunction = playFunction;
    }

    public static RockScissorsPaper fromInitial(String initial) {
        for (RockScissorsPaper rockScissorsPaper : values()) {
            if (rockScissorsPaper.initial.equals(initial)) {
                return rockScissorsPaper;
            }
        }
        throw new IllegalArgumentException();
    }

    public Winner getWinner(RockScissorsPaper rockScissorsPaper) {
        if (this.equals(rockScissorsPaper)) {
            return Winner.TIE;
        }
        return playFunction.apply(rockScissorsPaper.initial);
    }
}

enum Winner {
    PLAYER_1,
    PLAYER_2,
    TIE;
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
import java.util.function.Function;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideGameResults")
    @DisplayName("가위 바위 보 게임 결과가 주어졌을 때 승자를 반환한다.")
    void testGetWinner(List<String> gameResults, String expected) {
        Game sut = new Game();

        String actual = sut.getWinner(gameResults);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideGameResults() {
        return Stream.of(
                Arguments.of(List.of("R R"), "TIE"),
                Arguments.of(List.of("S S"), "TIE"),
                Arguments.of(List.of("P P"), "TIE"),
                Arguments.of(List.of("R S"), "Player 1"),
                Arguments.of(List.of("R P"), "Player 2"),
                Arguments.of(List.of("S P"), "Player 1"),
                Arguments.of(List.of("S R"), "Player 2"),
                Arguments.of(List.of("P R"), "Player 1"),
                Arguments.of(List.of("P S"), "Player 2"),
                Arguments.of(List.of("R S", "S R"), "TIE"),
                Arguments.of(List.of("R P", "S R"), "Player 2"),
                Arguments.of(List.of("P P", "R S", "S R"), "TIE"),
                Arguments.of(List.of("R S", "R S", "S R"), "Player 1")
        );
    }

    @ParameterizedTest
    @MethodSource("provideRockScissorsPapers")
    @DisplayName("가위 바위 보 게임 결과를 반환한다.")
    void testPlay(String gameResult, Winner expected) {
        RockScissorsPaperGame sut = RockScissorsPaperGame.from(gameResult);

        Winner actual = sut.play();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideRockScissorsPapers() {
        return Stream.of(
                Arguments.of("R R", Winner.TIE),
                Arguments.of("S S", Winner.TIE),
                Arguments.of("P P", Winner.TIE),
                Arguments.of("R S", Winner.PLAYER_1),
                Arguments.of("R P", Winner.PLAYER_2),
                Arguments.of("S P", Winner.PLAYER_1),
                Arguments.of("S R", Winner.PLAYER_2),
                Arguments.of("P R", Winner.PLAYER_1),
                Arguments.of("P S", Winner.PLAYER_2)
        );
    }
}

class Game {

    private int firstPlayerScore;
    private int secondPlayerScore;

    public String getWinner(List<String> gameResults) {
        playGame(gameResults);
        return getWinnerOfScores();
    }

    private String getWinnerOfScores() {
        if (firstPlayerScore == secondPlayerScore) {
            return "TIE";
        }
        return firstPlayerScore > secondPlayerScore ? "Player 1" : "Player 2";
    }

    private void playGame(List<String> gameResults) {
        for (String gameResult : gameResults) {
            RockScissorsPaperGame game = RockScissorsPaperGame.from(gameResult);
            plusWinnerScore(game.play());
        }
    }

    private void plusWinnerScore(Winner winner) {
        if (winner.equals(Winner.PLAYER_1)) {
            firstPlayerScore++;
        } else if (winner.equals(Winner.PLAYER_2)) {
            secondPlayerScore++;
        }
    }
}

class RockScissorsPaperGame {

    public static final String DELIMITER = " ";
    public static final int FIRST_PLAYER_SUBMIT_INDEX = 0;
    public static final int SECOND_PLAYER_SUBMIT_INDEX = 1;

    private final RockScissorsPaper firstPlayerSubmit;
    private final RockScissorsPaper secondPlayerSubmit;

    private RockScissorsPaperGame(RockScissorsPaper firstPlayerSubmit, RockScissorsPaper secondPlayerSubmit) {
        this.firstPlayerSubmit = firstPlayerSubmit;
        this.secondPlayerSubmit = secondPlayerSubmit;
    }

    public static RockScissorsPaperGame from(String submit) {
        String[] splitString = submit.split(DELIMITER);
        return new RockScissorsPaperGame(RockScissorsPaper.fromInitial(splitString[FIRST_PLAYER_SUBMIT_INDEX]),
                RockScissorsPaper.fromInitial(splitString[SECOND_PLAYER_SUBMIT_INDEX]));
    }

    public Winner play() {
        return firstPlayerSubmit.getWinner(secondPlayerSubmit);
    }
}

enum RockScissorsPaper {
    ROCK("R", submit -> submit.equals("S") ? Winner.PLAYER_1 : Winner.PLAYER_2),
    SCISSORS("S", submit -> submit.equals("P") ? Winner.PLAYER_1 : Winner.PLAYER_2),
    PAPER("P", submit -> submit.equals("R") ? Winner.PLAYER_1 : Winner.PLAYER_2);

    private final String initial;
    private final Function<String, Winner> playFunction;

    RockScissorsPaper(String initial, Function<String, Winner> playFunction) {
        this.initial = initial;
        this.playFunction = playFunction;
    }

    public static RockScissorsPaper fromInitial(String initial) {
        for (RockScissorsPaper rockScissorsPaper : values()) {
            if (rockScissorsPaper.initial.equals(initial)) {
                return rockScissorsPaper;
            }
        }
        throw new IllegalArgumentException();
    }

    public Winner getWinner(RockScissorsPaper rockScissorsPaper) {
        if (this.equals(rockScissorsPaper)) {
            return Winner.TIE;
        }
        return playFunction.apply(rockScissorsPaper.initial);
    }
}

enum Winner {
    PLAYER_1,
    PLAYER_2,
    TIE;
}
~~~
