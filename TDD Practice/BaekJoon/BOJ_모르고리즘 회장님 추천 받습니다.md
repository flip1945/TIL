# 모르고리즘 회장님 추천 받습니다 (Bronze 2)

### 문제 설명

국렬이는 모르고리즘 차기 회장을 빠르게 구해야 한다. 안 그러면 대학원 가서도 회장을 해야 하기 때문이다.

그래서 국렬이는 어떻게든 2020년 연세대학교 프로그래밍 경진대회를 열어서 차기 회장을 선택하려고 했으나, 코로나19 때문에 미루고 결국 11월에 개최하게 되었다.

국렬이는 대회를 치른 사람 중에서 점수가 가장 높은 사람을 억지로 차기 회장으로 지목하려고 한다. 만약에 가장 높은 사람이 2명 이상 있는 경우, 이름이 사전 순으로 가장 앞선 사람을 차기 회장으로 뽑을 것이다.

차기 회장으로 누가 지목될지 알아내라.

출처 : https://www.acmicpc.net/problem/20124

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> voteResult = inputVoteResult(scanner, n);

        VoteCounter voteCounter = new VoteCounter(voteResult);
        System.out.println(voteCounter.whoIsElected());
    }

    private static List<String> inputVoteResult(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class VoteCounter {

    private final List<Vote> votes;

    public VoteCounter(List<String> voteResult) {
        this.votes = initVotes(voteResult);
    }

    private List<Vote> initVotes(List<String> voteResult) {
        return voteResult.stream()
                .map(Vote::from)
                .collect(Collectors.toList());
    }

    public String whoIsElected() {
        return votes.stream()
                .sorted()
                .findFirst()
                .orElseThrow()
                .getName();
    }
}

class Vote implements Comparable<Vote> {

    private final String name;
    private final int count;

    private Vote(String name, int count) {
        this.name = name;
        this.count = count;
    }

    public static Vote from(String voteResult) {
        String[] split = voteResult.split(" ");
        return new Vote(split[0], Integer.parseInt(split[1]));
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(Vote o) {
        int result = Integer.compare(o.count, this.count);
        if (result != 0) {
            return result;
        }
        return this.name.compareTo(o.name);
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
    @MethodSource("provideVoteResults")
    @DisplayName("투표의 당선된 사람의 이름을 반환한다.")
    void schoolSongTest(List<String> voteResults, String expected) {
        VoteCounter sut = new VoteCounter(voteResults);

        String actual = sut.whoIsElected();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideVoteResults() {
        return Stream.of(
                Arguments.of(List.of("a 2", "b 1"), "a"),
                Arguments.of(List.of("a 1", "b 2"), "b"),
                Arguments.of(List.of("a 1", "b 2", "c 10"), "c"),
                Arguments.of(List.of("a 1"), "a"),
                Arguments.of(List.of("a 1", "b 10", "c 10"), "b"),
                Arguments.of(List.of("a 1", "c 10", "b 10"), "b"),
                Arguments.of(List.of("inseop 10", "gukryeol 1", "juno 11"), "juno"),
                Arguments.of(List.of("inseop 10", "gukryeol 10", "juno 10"), "gukryeol")
        );
    }
}

class VoteCounter {

    private final List<Vote> votes;

    public VoteCounter(List<String> voteResult) {
        this.votes = initVotes(voteResult);
    }

    private List<Vote> initVotes(List<String> voteResult) {
        return voteResult.stream()
                .map(Vote::from)
                .collect(Collectors.toList());
    }

    public String whoIsElected() {
        return votes.stream()
                .sorted()
                .findFirst()
                .orElseThrow()
                .getName();
    }
}

class Vote implements Comparable<Vote> {

    private final String name;
    private final int count;

    private Vote(String name, int count) {
        this.name = name;
        this.count = count;
    }

    public static Vote from(String voteResult) {
        String[] split = voteResult.split(" ");
        return new Vote(split[0], Integer.parseInt(split[1]));
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(Vote o) {
        int result = Integer.compare(o.count, this.count);
        if (result != 0) {
            return result;
        }
        return this.name.compareTo(o.name);
    }
}
~~~
