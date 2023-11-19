# Sport Climbing Combined (Silver 5)

### 문제 설명

스포츠 클라이밍은 1986 년에 시작된 실내 암벽 등반 스포츠이다. 선수들은 원래 리드 클라이밍 종목에서만 겨루었는데, 1989 년에 스피드 클라이밍이 추가되었고, 10 년 후인 1999 년에 볼더링 종목이 추가되었다. 올림픽 게임에서는 금, 은, 동메달을 결정하기 위하여 선수들은 세 종목에서 겨루어 종합 순위를 매긴다. 종합 순위는 세 종목에서 거둔 순위를 곱한 점수로 결정된다. 예를 들어, 어떤 선수가 리드에서 1 위, 스피드에서 5 위, 볼더링에서 2 위를 했다면 점수는 10 점이 된다. 곱한 점수가 낮은 선수가 종합 순위에서 앞선다.

선수 $n$명의 등번호와 이들이 세 종목에서 거둔 순위가 주어질 때, 금, 은, 동메달을 받을 선수를 결정하는 프로그램을 작성하시오. 두 선수의 곱한 점수가 같을 수도 있다. 이 경우, 세 종목 순위의 합산 점수가 낮은 선수가 이긴다. 두 선수의 곱한 점수와 합산 점수가 모두 같으면 등번호가 낮은 선수가 이긴다.

출처 : https://www.acmicpc.net/problem/23246

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> climbingResults = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());

        SportClimbingContest contest = SportClimbingContest.from(climbingResults);
        System.out.println(contest.winners());
    }
}

class SportClimbingContest {

    private static final int WINNER_SIZE = 3;
    private static final String DELIMITER = " ";

    private final List<Participant> participants;

    public SportClimbingContest(List<Participant> participants) {
        this.participants = participants;
    }

    public static SportClimbingContest from(List<String> climbingResults) {
        return new SportClimbingContest(initParticipants(climbingResults));
    }

    private static List<Participant> initParticipants(List<String> climbingResults) {
        return climbingResults.stream()
                .map(Participant::from)
                .collect(Collectors.toList());
    }

    public String winners() {
        return participants.stream()
                .sorted()
                .limit(WINNER_SIZE)
                .map(Participant::getId)
                .collect(Collectors.joining(DELIMITER));
    }
}

class Participant implements Comparable<Participant> {

    private static final String DELIMITER = " ";
    private static final int ID_INDEX = 0;
    private static final int LEAD_RANK_INDEX = 1;
    private static final int SPEED_RANK_INDEX = 2;
    private static final int BOULDERING_RANK_INDEX = 3;

    private final int id;
    private final int leadClimbingRank;
    private final int speedClimbingRank;
    private final int boulderingClimbingRank;

    public Participant(int id, int leadClimbingRank, int speedClimbingRank, int boulderingClimbingRank) {
        this.id = id;
        this.leadClimbingRank = leadClimbingRank;
        this.speedClimbingRank = speedClimbingRank;
        this.boulderingClimbingRank = boulderingClimbingRank;
    }

    public static Participant from(String climbingResult) {
        String[] splitResult = climbingResult.split(DELIMITER);
        int id = Integer.parseInt(splitResult[ID_INDEX]);
        int leadClimbingRank = Integer.parseInt(splitResult[LEAD_RANK_INDEX]);
        int speedClimbingRank = Integer.parseInt(splitResult[SPEED_RANK_INDEX]);
        int boulderingClimbingRank = Integer.parseInt(splitResult[BOULDERING_RANK_INDEX]);
        return new Participant(id, leadClimbingRank, speedClimbingRank, boulderingClimbingRank);
    }

    @Override
    public int compareTo(Participant o) {
        if (this.totalScore() != o.totalScore()) {
            return Integer.compare(this.totalScore(), o.totalScore());
        } else if (this.sumRank() != o.sumRank()) {
            return Integer.compare(this.sumRank(), o.sumRank());
        }
        return Integer.compare(this.id, o.id);
    }

    private int totalScore() {
        return leadClimbingRank * speedClimbingRank * boulderingClimbingRank;
    }

    private int sumRank() {
        return leadClimbingRank + speedClimbingRank + boulderingClimbingRank;
    }

    public String getId() {
        return String.valueOf(id);
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
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideClimbingResults")
    @DisplayName("스포츠 클라이밍 컨테스트는 우승자들을 반환한다.")
    void sportClimbingContestReturnsWinners(List<String> climbingResults, String expected) {
        SportClimbingContest sut = SportClimbingContest.from(climbingResults);

        String actual = sut.winners();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideClimbingResults() {
        return Stream.of(
                Arguments.of(List.of("1 1 1 1", "2 2 2 2", "3 3 3 3", "4 4 4 4"), "1 2 3"),
                Arguments.of(List.of("1 4 4 4", "2 2 2 2", "3 3 3 3", "4 1 1 1"), "4 2 3"),
                Arguments.of(List.of("1 1 1 4", "2 2 2 1", "3 3 3 3", "4 4 4 2"), "2 1 3"),
                Arguments.of(List.of("3 1 2 3", "2 2 3 1", "1 3 1 2"), "1 2 3"),
                Arguments.of(List.of("301 4 3 2", "815 2 2 1", "717 1 1 4", "505 3 4 2"), "815 717 301")
        );
    }
}

class SportClimbingContest {

    private static final int WINNER_SIZE = 3;
    private static final String DELIMITER = " ";

    private final List<Participant> participants;

    public SportClimbingContest(List<Participant> participants) {
        this.participants = participants;
    }

    public static SportClimbingContest from(List<String> climbingResults) {
        return new SportClimbingContest(initParticipants(climbingResults));
    }

    private static List<Participant> initParticipants(List<String> climbingResults) {
        return climbingResults.stream()
                .map(Participant::from)
                .collect(Collectors.toList());
    }

    public String winners() {
        return participants.stream()
                .sorted()
                .limit(WINNER_SIZE)
                .map(Participant::getId)
                .collect(Collectors.joining(DELIMITER));
    }
}

class Participant implements Comparable<Participant> {

    private static final String DELIMITER = " ";
    private static final int ID_INDEX = 0;
    private static final int LEAD_RANK_INDEX = 1;
    private static final int SPEED_RANK_INDEX = 2;
    private static final int BOULDERING_RANK_INDEX = 3;

    private final int id;
    private final int leadClimbingRank;
    private final int speedClimbingRank;
    private final int boulderingClimbingRank;

    public Participant(int id, int leadClimbingRank, int speedClimbingRank, int boulderingClimbingRank) {
        this.id = id;
        this.leadClimbingRank = leadClimbingRank;
        this.speedClimbingRank = speedClimbingRank;
        this.boulderingClimbingRank = boulderingClimbingRank;
    }

    public static Participant from(String climbingResult) {
        String[] splitResult = climbingResult.split(DELIMITER);
        int id = Integer.parseInt(splitResult[ID_INDEX]);
        int leadClimbingRank = Integer.parseInt(splitResult[LEAD_RANK_INDEX]);
        int speedClimbingRank = Integer.parseInt(splitResult[SPEED_RANK_INDEX]);
        int boulderingClimbingRank = Integer.parseInt(splitResult[BOULDERING_RANK_INDEX]);
        return new Participant(id, leadClimbingRank, speedClimbingRank, boulderingClimbingRank);
    }

    @Override
    public int compareTo(Participant o) {
        if (this.totalScore() != o.totalScore()) {
            return Integer.compare(this.totalScore(), o.totalScore());
        } else if (this.sumRank() != o.sumRank()) {
            return Integer.compare(this.sumRank(), o.sumRank());
        }
        return Integer.compare(this.id, o.id);
    }

    private int totalScore() {
        return leadClimbingRank * speedClimbingRank * boulderingClimbingRank;
    }

    private int sumRank() {
        return leadClimbingRank + speedClimbingRank + boulderingClimbingRank;
    }

    public String getId() {
        return String.valueOf(id);
    }
}
~~~
