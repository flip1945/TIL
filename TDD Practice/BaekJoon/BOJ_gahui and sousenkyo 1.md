# gahui and sousenkyo 1 (Bronze 4)

### 문제 설명

Gahui is watching the annual character election. After the election, The top 16 characters receive enormous benefits for one year because they belong to the Tier 1 (ranked 1st - 16th) section. For that reason, fans vote passionately to get their favorite characters into the top 16. Remarkably, at least one Cinderella appears in every election, achieving an outstanding outcome.

The competition rate of election is defined as the number of characters that satisfy the conditions below:

* The character **does not belong to the Tier 1** section.
* The difference in votes between the character and the 16th-ranked character is **$1\,000$ or less.**

Find the competition rate of the election.

출처 : https://www.acmicpc.net/problem/30791

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Integer> votes = List.of(scanner.nextInt(), scanner.nextInt(), scanner.nextInt(), scanner.nextInt(), scanner.nextInt());

        Tournament tournament = new Tournament(votes);
        System.out.println(tournament.countOfCompetitiveCandidates());
    }
}

class Tournament {
    private static final int CANDIDATE_FROM_INDEX = 1;
    private static final int COMPETITIVE_CANDIDATE_LIMIT = 1000;
    private static final int TIER_1_VOTES_INDEX = 0;

    private final List<Integer> votes;

    public Tournament(List<Integer> votes) {
        this.votes = votes;
    }

    public int countOfCompetitiveCandidates() {
        return (int) getCandidatesVotes().stream()
                .filter(this::isCompetitiveCandidate)
                .count();
    }

    private List<Integer> getCandidatesVotes() {
        return votes.subList(CANDIDATE_FROM_INDEX, votes.size());
    }

    private boolean isCompetitiveCandidate(int vote) {
        return getDifferenceFromTier1(vote) <= COMPETITIVE_CANDIDATE_LIMIT;
    }

    private int getDifferenceFromTier1(int vote) {
        return getVoteOfTier1() - vote;
    }

    private Integer getVoteOfTier1() {
        return votes.get(TIER_1_VOTES_INDEX);
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
    @MethodSource("provideVotesFrom16thTo20th")
    @DisplayName("16부터 20위의 투표수가 주어졌을 때 경쟁력 있는 후보의 숫자를 반환한다.")
    void tournamentReturnsCountOfCompetitiveCandidatesGivenVotesFrom16thTo20th(List<Integer> votes, int expected) {
        Tournament sut = new Tournament(votes);

        int actual = sut.countOfCompetitiveCandidates();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideVotesFrom16thTo20th() {
        return Stream.of(
                Arguments.of(List.of(40000, 39000, 38000, 37000, 36000), 1),
                Arguments.of(List.of(40060, 40000, 39800, 38996, 37737), 2),
                Arguments.of(List.of(43300, 39800, 38900, 36500, 33500), 0),
                Arguments.of(List.of(43300, 43100, 43000, 42955, 42300), 4)
        );
    }
}

class Tournament {
    private static final int CANDIDATE_FROM_INDEX = 1;
    private static final int COMPETITIVE_CANDIDATE_LIMIT = 1000;
    private static final int TIER_1_VOTES_INDEX = 0;

    private final List<Integer> votes;

    public Tournament(List<Integer> votes) {
        this.votes = votes;
    }

    public int countOfCompetitiveCandidates() {
        return (int) getCandidatesVotes().stream()
                .filter(this::isCompetitiveCandidate)
                .count();
    }

    private List<Integer> getCandidatesVotes() {
        return votes.subList(CANDIDATE_FROM_INDEX, votes.size());
    }

    private boolean isCompetitiveCandidate(int vote) {
        return getDifferenceFromTier1(vote) <= COMPETITIVE_CANDIDATE_LIMIT;
    }

    private int getDifferenceFromTier1(int vote) {
        return getVoteOfTier1() - vote;
    }

    private Integer getVoteOfTier1() {
        return votes.get(TIER_1_VOTES_INDEX);
    }
}
~~~
