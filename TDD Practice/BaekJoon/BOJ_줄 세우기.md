# 줄 세우기 (Silver 5)

### 문제 설명

악독한 코치 주혁은 선수들을 이름 순으로 세우는 것을 좋아한다. 더 악독한 것은 어떤 순서로 서야할지도 알려주지 않았다! 선수들의 이름이 주어질 때 어떤 순서로 이루어져있는지 확인해보자.

출처 : https://www.acmicpc.net/problem/11536

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        NameOrderChecker checker = new NameOrderChecker(inputNames(scanner, n));
        System.out.println(checker.getSortState());
    }

    private static List<String> inputNames(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class NameOrderChecker {
    private final List<String> names;

    public NameOrderChecker(List<String> names) {
        this.names = new ArrayList<>(names);
    }

    public String getSortState() {
        return getSortOrder().name();
    }

    private SortOrder getSortOrder() {
        if (isIncreasingOrder()) {
            return SortOrder.INCREASING;
        } else if (isDecreasingOrder()) {
            return SortOrder.DECREASING;
        }
        return SortOrder.NEITHER;
    }

    private boolean isIncreasingOrder() {
        List<String> increasingNames = names.stream()
                .sorted()
                .collect(Collectors.toList());
        return names.equals(increasingNames);
    }

    private boolean isDecreasingOrder() {
        List<String> decreasingNames = names.stream()
                .sorted(Comparator.reverseOrder())
                .collect(Collectors.toList());
        return names.equals(decreasingNames);
    }

    enum SortOrder {
        INCREASING, DECREASING, NEITHER
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
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNames")
    @DisplayName("이름의 정렬 상태를 반환한다.")
    void getSortStateTest(List<String> names, String expected) {
        NameOrderChecker sut = new NameOrderChecker(names);

        String actual = sut.getSortState();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNames() {
        return Stream.of(
                Arguments.of(List.of("AA", "BB", "CC"), "INCREASING"),
                Arguments.of(List.of("CC", "BB", "AA"), "DECREASING"),
                Arguments.of(List.of("BB", "AA"), "DECREASING"),
                Arguments.of(List.of("AA", "CC", "BB"), "NEITHER"),
                Arguments.of(List.of("JOE", "BOB", "ANDY", "AL", "ADAM"), "DECREASING"),
                Arguments.of(List.of("HOPE", "ALI", "BECKY", "JULIE", "MEGHAN", "LAUREN", "MORGAN", "CARLI", "MEGAN", "ALEX", "TOBIN"), "NEITHER"),
                Arguments.of(List.of("GEORGE", "JOHN", "PAUL", "RINGO"), "INCREASING")
        );
    }
}

class NameOrderChecker {
    private final List<String> names;

    public NameOrderChecker(List<String> names) {
        this.names = new ArrayList<>(names);
    }

    public String getSortState() {
        return getSortOrder().name();
    }

    private SortOrder getSortOrder() {
        if (isIncreasingOrder()) {
            return SortOrder.INCREASING;
        } else if (isDecreasingOrder()) {
            return SortOrder.DECREASING;
        }
        return SortOrder.NEITHER;
    }

    private boolean isIncreasingOrder() {
        List<String> increasingNames = names.stream()
                .sorted()
                .collect(Collectors.toList());
        return names.equals(increasingNames);
    }

    private boolean isDecreasingOrder() {
        List<String> decreasingNames = names.stream()
                .sorted(Comparator.reverseOrder())
                .collect(Collectors.toList());
        return names.equals(decreasingNames);
    }

    enum SortOrder {
        INCREASING, DECREASING, NEITHER
    }
}
~~~
