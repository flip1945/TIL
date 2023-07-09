# 임스와 함께하는 미니게임 (Silver 5)

### 문제 설명

임스가 미니게임을 같이할 사람을 찾고 있습니다.

플레이할 미니게임으로는 윷놀이 
$Y$, 같은 그림 찾기 
$F$, 원카드 
$O$가 있습니다. 각각 2, 3, 4 명이서 플레이하는 게임이며 인원수가 부족하면 게임을 시작할 수 없습니다.

사람들이 임스와 같이 플레이하기를 신청한 횟수 
$N$과 임스가 플레이할 게임의 종류가 주어질 때, 최대 몇 번이나 임스와 함께 게임을 플레이할 수 있는지 구하시오.

임스와 여러 번 미니게임을 플레이하고자 하는 사람이 있으나, 임스는 한 번 같이 플레이한 사람과는 다시 플레이하지 않습니다.

임스와 함께 플레이하고자 하는 사람 중 동명이인은 존재하지 않습니다. 임스와 lms0806은 서로 다른 인물입니다.

출처 : https://www.acmicpc.net/problem/25757

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());

        MiniGame miniGame = MiniGame.from(st.nextToken());
        System.out.println(miniGame.getMaxCountOfGamesPossible(inputPlayersOfRequest(br, n)));
    }

    private static List<String> inputPlayersOfRequest(BufferedReader br, int n) throws IOException {
        List<String> playersOfRequest = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            playersOfRequest.add(br.readLine());
        }
        return playersOfRequest;
    }
}

class MiniGame {

    private final GameType gameType;

    private MiniGame(GameType gameType) {
        this.gameType = gameType;
    }

    public static MiniGame from(String gameType) {
        return new MiniGame(GameType.valueOf(gameType));
    }

    public int getMaxCountOfGamesPossible(List<String> playersOfRequest) {
        int countOfPlayers = removeDuplicatedPlayers(playersOfRequest).size();
        return gameType.getPlayableGameCount(countOfPlayers);
    }

    private HashSet<String> removeDuplicatedPlayers(List<String> playersOfRequest) {
        return new HashSet<>(playersOfRequest);
    }
}

enum GameType {
    Y("윷놀이", countOfPlayers -> countOfPlayers),
    F("같은 그림 찾기", countOfPlayers -> countOfPlayers / 2),
    O("원카드", countOfPlayers -> countOfPlayers / 3);

    private final String description;
    private final Function<Integer, Integer> playableGameCount;

    GameType(String description, Function<Integer, Integer> playableGameCount) {
        this.description = description;
        this.playableGameCount = playableGameCount;
    }

    public int getPlayableGameCount(int countOfPlayers) {
        return playableGameCount.apply(countOfPlayers);
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
import java.util.function.Function;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePlayers")
    @DisplayName("최대 몇 번의 게임 플레이가 가능한 지 반환한다.")
    void getMaxCountOfGamesPossibleTest(String gameType, List<String> playersOfRequest, int expected) {
        MiniGame sut = MiniGame.from(gameType);

        int actual = sut.getMaxCountOfGamesPossible(playersOfRequest);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> providePlayers() {
        return Stream.of(
                Arguments.of("Y" ,List.of("a"), 1),
                Arguments.of("Y" ,List.of("a", "b"), 2),
                Arguments.of("Y" ,List.of("a", "a", "b"), 2),
                Arguments.of("Y" ,List.of(), 0),
                Arguments.of("F" ,List.of("a", "b"), 1),
                Arguments.of("F" ,List.of("a", "b", "c", "d"), 2),
                Arguments.of("F" ,List.of("a", "b", "a", "c", "d", "a"), 2),
                Arguments.of("F" ,List.of("a"), 0),
                Arguments.of("O" ,List.of("a", "b", "c", "d"), 1),
                Arguments.of("O" ,List.of("a", "b", "c", "d", "e", "f", "g", "h", "i"), 3),
                Arguments.of("O" ,List.of("a", "b", "c", "d", "e", "f", "g", "h", "a"), 2),
                Arguments.of("O" ,List.of("a", "b"), 0)
        );
    }
}

class MiniGame {

    private final GameType gameType;

    private MiniGame(GameType gameType) {
        this.gameType = gameType;
    }

    public static MiniGame from(String gameType) {
        return new MiniGame(GameType.valueOf(gameType));
    }

    public int getMaxCountOfGamesPossible(List<String> playersOfRequest) {
        int countOfPlayers = removeDuplicatedPlayers(playersOfRequest).size();
        return gameType.getPlayableGameCount(countOfPlayers);
    }

    private HashSet<String> removeDuplicatedPlayers(List<String> playersOfRequest) {
        return new HashSet<>(playersOfRequest);
    }
}

enum GameType {
    Y("윷놀이", countOfPlayers -> countOfPlayers),
    F("같은 그림 찾기", countOfPlayers -> countOfPlayers / 2),
    O("원카드", countOfPlayers -> countOfPlayers / 3);

    private final String description;
    private final Function<Integer, Integer> playableGameCount;

    GameType(String description, Function<Integer, Integer> playableGameCount) {
        this.description = description;
        this.playableGameCount = playableGameCount;
    }

    public int getPlayableGameCount(int countOfPlayers) {
        return playableGameCount.apply(countOfPlayers);
    }
}
~~~
