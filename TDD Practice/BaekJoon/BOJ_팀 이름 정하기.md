# 팀 이름 정하기 (Bronze 1)

### 문제 설명

연두는 프로그래밍 대회에 나갈 팀 이름을 정하려고 한다. 미신을 믿는 연두는 이환이에게 공식을 하나 받아왔고, 이 공식을 이용해 우승할 확률이 가장 높은 팀 이름을 찾으려고 한다.

이환이가 만든 공식은 사용하려면 먼저 다음 4가지 변수의 값을 계산해야 한다.

* L = 연두의 이름과 팀 이름에서 등장하는 L의 개수
* O = 연두의 이름과 팀 이름에서 등장하는 O의 개수
* V = 연두의 이름과 팀 이름에서 등장하는 V의 개수
* E = 연두의 이름과 팀 이름에서 등장하는 E의 개수

그 다음, 위에서 구한 변수를 다음 식에 입력하면 팀 이름의 우승할 확률을 구할 수 있다.

((L+O) × (L+V) × (L+E) × (O+V) × (O+E) × (V+E)) mod 100

연두의 영어 이름과 팀 이름 후보 N개가 주어졌을 때, 우승할 확률이 가장 높은 팀 이름을 구해보자. 확률이 가장 높은 팀이 여러가지인 경우 사전 순으로 가장 앞서는 팀 이름이 우승할 확률이 가장 높은 것이다.

출처 : https://www.acmicpc.net/problem/1296

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String playerName = scanner.nextLine();
        int n = Integer.parseInt(scanner.nextLine());
        List<String> teamNames = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());

        ChanceOfWinningCalculator calculator = new ChanceOfWinningCalculator(teamNames);
        System.out.println(calculator.getTeamNameOfHighestChanceOfWinning(playerName));
    }
}

class ChanceOfWinningCalculator {
    private final List<String> teamNames;

    public ChanceOfWinningCalculator(List<String> teamNames) {
        this.teamNames = teamNames;
    }

    public String getTeamNameOfHighestChanceOfWinning(String playerName) {
        return teamNames.stream()
                .map(teamNames -> new TeamNameScore(teamNames, playerName))
                .sorted()
                .findFirst()
                .orElseGet(() -> new TeamNameScore("", ""))
                .getTeamName();
    }
}

class TeamNameScore implements Comparable<TeamNameScore> {
    private final String teamName;
    private final int countOfL;
    private final int countOfO;
    private final int countOfV;
    private final int countOfE;

    public TeamNameScore(String teamName, String playerName) {
        this.teamName = teamName;
        this.countOfL = getCount(playerName, 'L') + getCount(teamName, 'L');
        this.countOfO = getCount(playerName, 'O') + getCount(teamName, 'O');
        this.countOfV = getCount(playerName, 'V') + getCount(teamName, 'V');
        this.countOfE = getCount(playerName, 'E') + getCount(teamName, 'E');
    }

    public String getTeamName() {
        return teamName;
    }

    private int getScore() {
        return ((countOfL + countOfO) * (countOfL + countOfV) * (countOfL + countOfE) *
                (countOfO + countOfV) * (countOfO + countOfE) * (countOfV + countOfE)) % 100;
    }

    private int getCount(String name, char letter) {
        return (int) name.chars()
                .filter(ch -> ch == letter)
                .count();
    }

    @Override
    public int compareTo(TeamNameScore o) {
        if (this.getScore() > o.getScore()) {
            return -1;
        } else if (this.getScore() < o.getScore()) {
            return 1;
        }
        return this.teamName.compareTo(o.teamName);
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

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideTeamNamesAndPlayerName")
    @DisplayName("우승할 확률이 가장 높은 팀 이름을 반환한다.")
    void this_returns_team_name_with_the_highest_chance_of_winning(String playerName, List<String> teamNames, String expected) {
        ChanceOfWinningCalculator sut = new ChanceOfWinningCalculator(teamNames);

        String actual = sut.getTeamNameOfHighestChanceOfWinning(playerName);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideTeamNamesAndPlayerName() {
        return Stream.of(
                Arguments.of("A", List.of("A", "B", "L"), "A"),
                Arguments.of("A", List.of("A", "B", "O"), "A"),
                Arguments.of("A", List.of("AA", "B", "O"), "AA"),
                Arguments.of("JANE", List.of("THOMAS", "MICHAEL", "INDY", "LIU"), "INDY"),
                Arguments.of("LILLY", List.of("PIERRE"), "PIERRE"),
                Arguments.of("MERYLOV", List.of("JOHN", "DAVE", "STEVE", "JOHN", "DAVE"), "DAVE"),
                Arguments.of("LLOL", List.of("BVERON", "CVERON", "AVERON", "DVERON"), "AVERON"),
                Arguments.of("VELYLEOCEVE", List.of("YVXHOVE", "LCOKO", "OGWSJVEVEDLE", "WGFVSJEL", "VLOLUVCBLLQVESWHEEKC"), "VLOLUVCBLLQVESWHEEKC"),
                Arguments.of("LOVE", List.of("JACOB", "FRANK", "DANO"), "FRANK")
        );
    }
}

class ChanceOfWinningCalculator {
    private final List<String> teamNames;

    public ChanceOfWinningCalculator(List<String> teamNames) {
        this.teamNames = teamNames;
    }

    public String getTeamNameOfHighestChanceOfWinning(String playerName) {
        return teamNames.stream()
                .map(teamNames -> new TeamNameScore(teamNames, playerName))
                .sorted()
                .findFirst()
                .orElseGet(() -> new TeamNameScore("", ""))
                .getTeamName();
    }
}

class TeamNameScore implements Comparable<TeamNameScore> {
    private final String teamName;
    private final int countOfL;
    private final int countOfO;
    private final int countOfV;
    private final int countOfE;

    public TeamNameScore(String teamName, String playerName) {
        this.teamName = teamName;
        this.countOfL = getCount(playerName, 'L') + getCount(teamName, 'L');
        this.countOfO = getCount(playerName, 'O') + getCount(teamName, 'O');
        this.countOfV = getCount(playerName, 'V') + getCount(teamName, 'V');
        this.countOfE = getCount(playerName, 'E') + getCount(teamName, 'E');
    }

    public String getTeamName() {
        return teamName;
    }

    private int getScore() {
        return ((countOfL + countOfO) * (countOfL + countOfV) * (countOfL + countOfE) *
                (countOfO + countOfV) * (countOfO + countOfE) * (countOfV + countOfE)) % 100;
    }

    private int getCount(String name, char letter) {
        return (int) name.chars()
                .filter(ch -> ch == letter)
                .count();
    }

    @Override
    public int compareTo(TeamNameScore o) {
        if (this.getScore() > o.getScore()) {
            return -1;
        } else if (this.getScore() < o.getScore()) {
            return 1;
        }
        return this.teamName.compareTo(o.teamName);
    }
}
~~~
