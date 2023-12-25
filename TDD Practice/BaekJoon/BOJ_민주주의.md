# 민주주의 (Bronze 4)

### 문제 설명

월간 향유회에서는 민주주의적 다수결 투표 방식으로 문제의 출제 여부를 정한다. 즉, $N$개의 문제 후보마다 
$M$명의 출제위원이 찬반 의견을 내고, 과반수의 찬성을 얻은 문제가 출제된다. 이때 $M$은 항상 홀수이다.

문제 후보에 대한 출제위원의 찬반 의견이 주어졌을 때, 출제될 문제의 수를 구하여라.

출처 : https://www.acmicpc.net/problem/30999

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        int n = Integer.parseInt(st.nextToken());
        List<String> voteResult = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());

        DemocracyProblems democracyProblems = new DemocracyProblems(voteResult);
        System.out.println(democracyProblems.countOfSubmitProblems());
    }
}

class DemocracyProblems {
    private static final char YES_VOTE = 'O';
    private static final char NO_VOTE = 'X';

    private final List<String> voteResults;

    public DemocracyProblems(List<String> voteResults) {
        this.voteResults = voteResults;
    }

    public int countOfSubmitProblems() {
        return (int) voteResults.stream()
                .filter(this::isSubmitProblem)
                .count();
    }

    private boolean isSubmitProblem(String voteResult) {
        return getYesVoteCount(voteResult) > getNoVoteCount(voteResult);
    }

    private int getYesVoteCount(String voteResult) {
        return (int) IntStream.range(0, voteResult.length())
                .filter(i -> voteResult.charAt(i) == YES_VOTE)
                .count();
    }

    private int getNoVoteCount(String voteResult) {
        return (int) IntStream.range(0, voteResult.length())
                .filter(i -> voteResult.charAt(i) == NO_VOTE)
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
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideVoteResults")
    @DisplayName("투표 결과들이 주어지면 출제할 문제의 수를 반환한다.")
    void democracyProblemsReturnsCountOfSubmitProblems(List<String> voteResults, int expected) {
        DemocracyProblems sut = new DemocracyProblems(voteResults);

        int actual = sut.countOfSubmitProblems();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideVoteResults() {
        return Stream.of(
                Arguments.of(List.of("OXX"), 0),
                Arguments.of(List.of("OOX"), 1),
                Arguments.of(List.of("OOX", "OXX"), 1),
                Arguments.of(List.of("OOXXO", "XXXXX", "OOOOO"), 2)
        );
    }
}

class DemocracyProblems {
    private static final char YES_VOTE = 'O';
    private static final char NO_VOTE = 'X';

    private final List<String> voteResults;

    public DemocracyProblems(List<String> voteResults) {
        this.voteResults = voteResults;
    }

    public int countOfSubmitProblems() {
        return (int) voteResults.stream()
                .filter(this::isSubmitProblem)
                .count();
    }

    private boolean isSubmitProblem(String voteResult) {
        return getYesVoteCount(voteResult) > getNoVoteCount(voteResult);
    }

    private int getYesVoteCount(String voteResult) {
        return (int) IntStream.range(0, voteResult.length())
                .filter(i -> voteResult.charAt(i) == YES_VOTE)
                .count();
    }

    private int getNoVoteCount(String voteResult) {
        return (int) IntStream.range(0, voteResult.length())
                .filter(i -> voteResult.charAt(i) == NO_VOTE)
                .count();
    }
}
~~~
