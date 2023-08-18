# 개표 (Bronze 3)

### 문제 설명

A와 B가 한 오디션 프로의 결승전에 진출했다. 결승전의 승자는 심사위원의 투표로 결정된다.

심사위원의 투표 결과가 주어졌을 때, 어떤 사람이 우승하는지 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/10102

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();
        VoteCounter counter = new VoteCounter(scanner.nextLine());
        System.out.println(counter.count());
    }
}

class VoteCounter {

    public static final String DELIMITER = "";
    public static final String PARTICIPANT_A = "A";
    public static final String PARTICIPANT_B = "B";
    public static final String TIE = "Tie";

    private final String votes;

    public VoteCounter(String votes) {
        this.votes = votes;
    }

    public String count() {
        return getWinner(countOf(PARTICIPANT_A), countOf(PARTICIPANT_B));
    }

    private String getWinner(int aCount, int bCount) {
        if (aCount == bCount) {
            return TIE;
        }
        return aCount > bCount ? PARTICIPANT_A : PARTICIPANT_B;
    }

    private int countOf(String participant) {
        return (int) Arrays.stream(votes.split(DELIMITER))
                .filter(vote -> vote.equals(participant))
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

import java.util.Arrays;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideVotes")
    @DisplayName("투표 결과가 주어졌을 때 개표해야 한다.")
    void shouldCountVotes(String votes, String expected) {
        VoteCounter sut = new VoteCounter(votes);

        String actual = sut.count();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideVotes() {
        return Stream.of(
                Arguments.of("A", "A"),
                Arguments.of("B", "B"),
                Arguments.of("AB", "Tie"),
                Arguments.of("AAB", "A"),
                Arguments.of("AABB", "Tie"),
                Arguments.of("ABBABB", "B")
        );
    }
}

class VoteCounter {

    public static final String DELIMITER = "";
    public static final String PARTICIPANT_A = "A";
    public static final String PARTICIPANT_B = "B";
    public static final String TIE = "Tie";

    private final String votes;

    public VoteCounter(String votes) {
        this.votes = votes;
    }

    public String count() {
        return getWinner(countOf(PARTICIPANT_A), countOf(PARTICIPANT_B));
    }

    private String getWinner(int aCount, int bCount) {
        if (aCount == bCount) {
            return TIE;
        }
        return aCount > bCount ? PARTICIPANT_A : PARTICIPANT_B;
    }

    private int countOf(String participant) {
        return (int) Arrays.stream(votes.split(DELIMITER))
                .filter(vote -> vote.equals(participant))
                .count();
    }
}
~~~
