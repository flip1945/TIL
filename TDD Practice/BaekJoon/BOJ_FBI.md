# FBI (Bronze 3)

### 문제 설명

5명의 요원 중 FBI 요원을 찾는 프로그램을 작성하시오.

FBI요원은 요원의 첩보원명에 FBI가 들어있다. 

출처 : https://www.acmicpc.net/problem/2857

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<String> agents = inputAgents(scanner);

        FBIAgents fBIAgents = FBIAgents.from(agents);
        System.out.println(fBIAgents.getAgentNumbersOfFBI());
    }

    private static List<String> inputAgents(Scanner scanner) {
        return IntStream.range(0, 5)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class FBIAgents {

    private static final String FBI_AGENT = "FBI";
    private static final String DEFAULT = "HE GOT AWAY!";
    private static final String DELIMITER = " ";

    private final List<String> agents;

    private FBIAgents(List<String> agents) {
        this.agents = agents;
    }

    public static FBIAgents from(List<String> agents) {
        return new FBIAgents(new ArrayList<>(agents));
    }

    public String getAgentNumbersOfFBI() {
        List<Integer> agentNumbers = getAgentNumbers();
        return getAgentNumbersOfFBIOrDefault(agentNumbers);
    }

    private List<Integer> getAgentNumbers() {
        return IntStream.range(0, agents.size())
                .filter(i -> agents.get(i).contains(FBI_AGENT))
                .mapToObj(i -> i + 1)
                .collect(Collectors.toList());
    }

    private String getAgentNumbersOfFBIOrDefault(List<Integer> agentNumbers) {
        if (!agentNumbers.isEmpty()) {
            return joiningAgentNumbers(agentNumbers);
        }
        return DEFAULT;
    }

    private String joiningAgentNumbers(List<Integer> numbers) {
        return numbers.stream()
                .map(String::valueOf)
                .collect(Collectors.joining(DELIMITER));
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

import java.util.ArrayList;
import java.util.List;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideAgents")
    @DisplayName("5명의 요원 중 FBI 요원의 번호를 반환한다.")
    void shouldReturnNumbersOfFBIAgent(List<String> agents, String expected) {
        FBIAgents sut = FBIAgents.from(agents);

        String actual = sut.getAgentNumbersOfFBI();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideAgents() {
        return Stream.of(
                Arguments.of(List.of("FBI", "A", "B", "C", "D"), "1"),
                Arguments.of(List.of("A", "FBI", "B", "C", "D"), "2"),
                Arguments.of(List.of("A", "B", "FBI", "C", "D"), "3"),
                Arguments.of(List.of("A", "B", "C", "FBI", "D"), "4"),
                Arguments.of(List.of("A", "B", "C", "D", "FBI"), "5"),
                Arguments.of(List.of("FBI", "FBI", "C", "D", "E"), "1 2"),
                Arguments.of(List.of("FBI", "FBI", "FBI", "FBI", "FBI"), "1 2 3 4 5"),
                Arguments.of(List.of("AFBIB", "FBI", "FBI", "FBI", "FBI"), "1 2 3 4 5"),
                Arguments.of(List.of("A", "B", "C", "D", "E"), "HE GOT AWAY!")
        );
    }
}

class FBIAgents {

    private static final String FBI_AGENT = "FBI";
    private static final String DEFAULT = "HE GOT AWAY!";
    private static final String DELIMITER = " ";

    private final List<String> agents;

    private FBIAgents(List<String> agents) {
        this.agents = agents;
    }

    public static FBIAgents from(List<String> agents) {
        return new FBIAgents(new ArrayList<>(agents));
    }

    public String getAgentNumbersOfFBI() {
        List<Integer> agentNumbers = getAgentNumbers();
        return getAgentNumbersOfFBIOrDefault(agentNumbers);
    }

    private List<Integer> getAgentNumbers() {
        return IntStream.range(0, agents.size())
                .filter(i -> agents.get(i).contains(FBI_AGENT))
                .mapToObj(i -> i + 1)
                .collect(Collectors.toList());
    }

    private String getAgentNumbersOfFBIOrDefault(List<Integer> agentNumbers) {
        if (!agentNumbers.isEmpty()) {
            return joiningAgentNumbers(agentNumbers);
        }
        return DEFAULT;
    }

    private String joiningAgentNumbers(List<Integer> numbers) {
        return numbers.stream()
                .map(String::valueOf)
                .collect(Collectors.joining(DELIMITER));
    }
}
~~~
