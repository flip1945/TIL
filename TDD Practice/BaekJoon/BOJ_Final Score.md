# Final Score (Silver 4)

### 문제 설명

We have had a problem with one of our hard disks and we lost the final score of some football matches. However, we have been able to recover the names of the players that scored and found the members of each team on Wikipedia.

출처 : https://www.acmicpc.net/problem/15233

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();

        Team firstTeam = new Team(inputPlayers(scanner.nextLine()));
        Team secondTeam = new Team(inputPlayers(scanner.nextLine()));
        List<String> scorers = inputScorers(scanner.nextLine());

        Match match = new Match(firstTeam, secondTeam);

        System.out.println(match.winner(scorers));
    }

    private static Set<String> inputPlayers(String players) {
        return Arrays.stream(players.split(" "))
                .collect(Collectors.toSet());
    }

    private static List<String> inputScorers(String scorers) {
        return Arrays.stream(scorers.split(" "))
                .collect(Collectors.toList());
    }
}

class Match {

    private static final String DRAW = "TIE";
    private static final String A_TEAM_WIN = "A";
    private static final String B_TEAM_WIN = "B";

    private final Team aTeam;
    private final Team bTeam;

    public Match(Team aTeam, Team bTeam) {
        this.aTeam = aTeam;
        this.bTeam = bTeam;
    }

    public String winner(List<String> scorers) {
        int aTeamScore = aTeam.score(scorers);
        int bTeamScore = bTeam.score(scorers);

        if (aTeamScore == bTeamScore) {
            return DRAW;
        }
        return (aTeamScore > bTeamScore) ? A_TEAM_WIN : B_TEAM_WIN;
    }
}

class Team {

    private final Set<String> players;

    public Team(Set<String> players) {
        this.players = players;
    }

    public int score(List<String> scorers) {
        return (int) scorers.stream()
                .filter(players::contains)
                .count();
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
import java.util.Set;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePlayersAndScorers")
    @DisplayName("선수 명단과 득점 선수 명단이 주어지면 득점 수를 반환한다.")
    void teamReturnsTeamScoreGivenPlayersAndScorers(Set<String> players, List<String> scorers, int expected) {
        Team sut = new Team(players);

        int actual = sut.score(scorers);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> providePlayersAndScorers() {
        return Stream.of(
                Arguments.of(Set.of("a", "b", "c"), List.of("a"), 1),
                Arguments.of(Set.of("a", "b", "c"), List.of("a", "a"), 2),
                Arguments.of(Set.of("a", "b", "c"), List.of("d", "d", "d"), 0)
        );
    }

    @ParameterizedTest
    @MethodSource("provideTwoTeams")
    @DisplayName("두 팀의 선수 명단과 득점 선수 명단이 주어지면 이긴 팀을 반환한다.")
    void teamReturnsTeamScoreGivenPlayersAndScorers(Team aTeam, Team bTeam, List<String> scorers, String expected) {
        Match sut = new Match(aTeam, bTeam);

        String actual = sut.winner(scorers);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideTwoTeams() {
        return Stream.of(
                Arguments.of(
                        new Team(Set.of("messi", "neymar", "suarez")),
                        new Team(Set.of("ronaldo", "bale", "james")),
                        List.of("messi", "messi", "messi", "bale", "james"),
                        "A"
                ), Arguments.of(
                        new Team(Set.of("jordan", "pippen", "oneal", "bryant")),
                        new Team(Set.of("villa")),
                        List.of("villa", "villa", "villa", "villa", "villa"),
                        "B"
                ), Arguments.of(
                        new Team(Set.of("messi", "neymar", "suarez")),
                        new Team(Set.of("ronaldo", "bale", "james")),
                        List.of("bale", "messi", "ronaldo", "suarez"),
                        "TIE"
                )
        );
    }
}

class Match {

    private static final String DRAW = "TIE";
    private static final String A_TEAM_WIN = "A";
    private static final String B_TEAM_WIN = "B";

    private final Team aTeam;
    private final Team bTeam;

    public Match(Team aTeam, Team bTeam) {
        this.aTeam = aTeam;
        this.bTeam = bTeam;
    }

    public String winner(List<String> scorers) {
        int aTeamScore = aTeam.score(scorers);
        int bTeamScore = bTeam.score(scorers);

        if (aTeamScore == bTeamScore) {
            return DRAW;
        }
        return (aTeamScore > bTeamScore) ? A_TEAM_WIN : B_TEAM_WIN;
    }
}

class Team {

    private final Set<String> players;

    public Team(Set<String> players) {
        this.players = players;
    }

    public int score(List<String> scorers) {
        return (int) scorers.stream()
                .filter(players::contains)
                .count();
    }
}
~~~
