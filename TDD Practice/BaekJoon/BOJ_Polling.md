# Polling (Silver 5)

### 문제 설명

Midterm elections are here! Help your local election commission by counting votes and telling them the winner. If more than one candidate ties with the most votes, print out all of their names in alphabetical order.

출처 : https://www.acmicpc.net/problem/11235

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        Polling polling = new Polling(inputVotes(scanner, n));
        polling.mostVotesCandidates().forEach(System.out::println);
    }

    private static List<String> inputVotes(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Polling {

    private final List<String> votes;

    public Polling(List<String> votes) {
        this.votes = votes;
    }

    public List<String> mostVotesCandidates() {
        return mostVotesCandidates(getMaxVoteCount());
    }

    private List<String> mostVotesCandidates(long maxVoteCount) {
        return getVoteResultByName().entrySet().stream()
                .filter(entry -> entry.getValue() == maxVoteCount)
                .map(Map.Entry::getKey)
                .collect(Collectors.toList());
    }

    private TreeMap<String, Long> getVoteResultByName() {
        return votes.stream()
                .collect(Collectors.groupingBy(Function.identity(), TreeMap::new, Collectors.counting()));
    }

    private Long getMaxVoteCount() {
        return getVoteResultByName().values()
                .stream()
                .max(Comparator.naturalOrder())
                .orElse(Long.MIN_VALUE);
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
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideVotes")
    @DisplayName("투표는 최다 득점자를 반환한다.")
    void pollingReturnsMostVotesCandidate(List<String> votes, List<String> expected) {
        Polling sut = new Polling(votes);
        
        List<String> actual = sut.mostVotesCandidates();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideVotes() {
        return Stream.of(
                Arguments.of(List.of("AA", "BB", "AA"), List.of("AA")),
                Arguments.of(List.of("AA", "BB", "BB"), List.of("BB")),
                Arguments.of(List.of("FRED", "BARNEY", "FRED", "FRED", "BARNEY"), List.of("FRED")),
                Arguments.of(List.of("PORTHOS", "ATHOS", "ARAMIS", "PORTHOS", "ATHOS"), List.of("ATHOS", "PORTHOS")),
                Arguments.of(List.of("CC", "BB", "AA"), List.of("AA", "BB", "CC"))
        );
    }
}

class Polling {

    private final List<String> votes;

    public Polling(List<String> votes) {
        this.votes = votes;
    }

    public List<String> mostVotesCandidates() {
        return mostVotesCandidates(getMaxVoteCount());
    }

    private List<String> mostVotesCandidates(long maxVoteCount) {
        return getVoteResultByName().entrySet().stream()
                .filter(entry -> entry.getValue() == maxVoteCount)
                .map(Map.Entry::getKey)
                .collect(Collectors.toList());
    }

    private TreeMap<String, Long> getVoteResultByName() {
        return votes.stream()
                .collect(Collectors.groupingBy(Function.identity(), TreeMap::new, Collectors.counting()));
    }

    private Long getMaxVoteCount() {
        return getVoteResultByName().values()
                .stream()
                .max(Comparator.naturalOrder())
                .orElse(Long.MIN_VALUE);
    }
}
~~~
