# 첼시를 도와줘 (Bronze 2)

### 문제 설명

구단이 성적을 내지 못한다면 답은 새 선수 영입뿐이다. 이것은 오늘날 유럽 리그에서 가장 흔한 전략이고, 노르웨이의 로젠버그 팀은 이러한 전략이 성공한 대표적 예시다. 그들은 많은 스카우터들을 지구 곳곳에 파견해 가능성 있는 루키를 찾는다.

현재 첼시는 프리미어 리그에서 헤매고 있고, 결국 새로운 선수를 사기로 결정했다. 하지만 그들은 스카우터를 기다리기 지쳤고, 훨씬 더 효율적인 전략을 개발해냈다. "만약 무언가 팔리고 있다면, 그것에는 합당한 이유가 있다"는 배룸의 명언이 바로 그것이다. 축구에서 이 말은 곧 가장 비싼 선수가 가장 좋은 선수라는 이야기가 된다. 

이에 따라 새로운 선수를 찾는 방법은 단순히 구단들에게 전화를 걸어 그들의 가장 비싼 선수를 사는게 되었다. 당신의 임무는 첼시가 리스트에서 가장 비싼 선수를 찾아낼 수 있도록 돕는 것이다.

출처 : https://www.acmicpc.net/problem/11098

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
            List<String> playerInfos = inputPlayerInfos(scanner, n);

            Team team = Team.from(playerInfos);
            System.out.println(team.mostExpensivePlayer());
        }
    }

    private static List<String> inputPlayerInfos(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(j -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Team {

    private final List<Player> players;

    private Team(List<Player> players) {
        this.players = players;
    }

    public static Team from(List<String> playerInfos) {
        return new Team(initPlayers(playerInfos));
    }

    private static List<Player> initPlayers(List<String> playerInfos) {
        return playerInfos.stream()
                .map(Player::from)
                .collect(Collectors.toList());
    }

    public String mostExpensivePlayer() {
        return players.stream()
                .sorted()
                .findFirst()
                .map(Player::getName)
                .orElseThrow();
    }
}

class Player implements Comparable<Player> {

    public static final String DELIMITER = " ";
    public static final int SALARY_INDEX = 0;
    public static final int NAME_INDEX = 1;

    private final String name;
    private final int salary;

    private Player(String name, int salary) {
        this.name = name;
        this.salary = salary;
    }

    public static Player from(String playerInfo) {
        String[] split = playerInfo.split(DELIMITER);
        return new Player(split[NAME_INDEX], Integer.parseInt(split[SALARY_INDEX]));
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(Player o) {
        return Integer.compare(o.salary, this.salary);
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
    @MethodSource("providePlayerInfos")
    @DisplayName("가장 비싼 선수의 이름을 출력한다.")
    void mostExpensivePlayerTest(List<String> playerInfos, String expected) {
        Team sut = Team.from(playerInfos);

        String actual = sut.mostExpensivePlayer();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> providePlayerInfos() {
        return Stream.of(
                Arguments.of(List.of("1 a"), "a"),
                Arguments.of(List.of("1 a", "2 b"), "b"),
                Arguments.of(List.of("2 a", "1 b"), "a"),
                Arguments.of(List.of("2 a", "1 b", "100 c"), "c"),
                Arguments.of(List.of("10 Iversen", "1000000 Nannskog", "2000000 Ronaldinho"), "Ronaldinho"),
                Arguments.of(List.of("1000000 Maradona", "999999 Batistuta"), "Maradona")
        );
    }
}

class Team {

    private final List<Player> players;

    private Team(List<Player> players) {
        this.players = players;
    }

    public static Team from(List<String> playerInfos) {
        return new Team(initPlayers(playerInfos));
    }

    private static List<Player> initPlayers(List<String> playerInfos) {
        return playerInfos.stream()
                .map(Player::from)
                .collect(Collectors.toList());
    }

    public String mostExpensivePlayer() {
        return players.stream()
                .sorted()
                .findFirst()
                .map(Player::getName)
                .orElseThrow();
    }
}

class Player implements Comparable<Player> {

    public static final String DELIMITER = " ";
    public static final int SALARY_INDEX = 0;
    public static final int NAME_INDEX = 1;

    private final String name;
    private final int salary;

    private Player(String name, int salary) {
        this.name = name;
        this.salary = salary;
    }

    public static Player from(String playerInfo) {
        String[] split = playerInfo.split(DELIMITER);
        return new Player(split[NAME_INDEX], Integer.parseInt(split[SALARY_INDEX]));
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(Player o) {
        return Integer.compare(o.salary, this.salary);
    }
}
~~~
