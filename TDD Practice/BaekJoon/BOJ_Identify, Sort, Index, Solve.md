# Identify, Sort, Index, Solve (Silver 5)

### 문제 설명

ISIS Puzzle은 "Identify, Sort, Index, Solve"의 절차로 푸는 퍼즐을 통칭한다.

퍼즐을 좋아하는 하이비는 HCPC에 아래와 같은 ISIS 퍼즐 문제를 내기로 했다.

* $ N $개의 문자열 $ S_1, S_2, \ldots, S_N $이 주어진다.
* Identify: 각 문자열과 대응되는 문제의 제목을 알아낸 뒤, 그 문제의 번호 $ I_i $와 난이도 $ D_i $를 알아낸다.
* Sort: 문제들을 번호 $ I_i $의 오름차순으로 정렬한다.
* Index: 각 문제 이름 $ S_i $에서 $ D_i $번째의 글자를 추출한다. 이때 추출된 글자가 소문자라면 대문자로 변환한다.
* Solve: Index 단계에서 추출한 글자들을 Sort 단계에서 정렬한 순서대로 나열한다.

하지만 Identify는 구현이 어려울 것이라고 생각해, Identify까지 완료된 자료를 주기로 했다.

Identify가 완료된 자료가 주어질 때, Sort, Index, Solve까지 완료한 뒤 나오는 문자열을 출력해보자.

출처 : https://www.acmicpc.net/problem/26150

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        Puzzle puzzle = Puzzle.from(inputProblems(scanner, n));
        System.out.println(puzzle.getAnswer());
    }

    private static List<String> inputProblems(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Puzzle {

    private final List<Problem> problems;

    public Puzzle(List<Problem> problems) {
        this.problems = problems;
    }

    public static Puzzle from(List<String> problems) {
        return new Puzzle(initProblems(problems));
    }

    private static List<Problem> initProblems(List<String> problems) {
        return problems.stream()
                .map(Problem::from)
                .collect(Collectors.toList());
    }

    public String getAnswer() {
        return problems.stream()
                .sorted()
                .map(Problem::getAnswer)
                .collect(Collectors.joining());
    }
}

class Problem implements Comparable<Problem> {

    private final String content;
    private final int number;
    private final int index;

    public Problem(String content, int number, int index) {
        this.content = content;
        this.number = number;
        this.index = index;
    }

    public static Problem from(String problem) {
        String[] split = problem.split(" ");
        return new Problem(split[0], Integer.parseInt(split[1]), Integer.parseInt(split[2]));
    }

    public String getAnswer() {
        return content.substring(index - 1, index).toUpperCase();
    }

    @Override
    public int compareTo(Problem o) {
        return Integer.compare(number, o.number);
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
    @MethodSource("providePuzzles")
    @DisplayName("퍼즐의 정답을 반환한다.")
    void puzzleReturnsAnswer(List<String> problems, String expected) {
        Puzzle sut = Puzzle.from(problems);

        String actual = sut.getAnswer();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> providePuzzles() {
        return Stream.of(
                Arguments.of(List.of("abc 1 1", "abc 2 2", "abc 3 3"), "ABC"),
                Arguments.of(List.of("abc 1 1", "def 2 2", "ghi 3 3"), "AEI"),
                Arguments.of(List.of("apple 1 5", "bat 2 2", "cat 3 3"), "EAT"),
                Arguments.of(List.of("Ep2ascii 7 3", "Xtreme2s 5 7", "AndAHalf 1 5", "May2Year 8 4", "PCMaudio 3 1",
                        "Logicism 2 5", "Electric 4 8", "2048Half 6 2"), "HCPC2022"),
                Arguments.of(List.of("BearAndThreeMusketeers 1661 9", "GlebAndPizza 1441 7",
                        "LittleElephantAndInversions 2662 19", "NewYearBookReading 1991 13",
                        "PalindromesColoring 2332 16", "PashmakAndParmidasProblem 2442 15",
                        "SendingASequenceOverTheNetwork 3003 7", "TextVolume 1221 10", "TheHardWorkOfPaparazzi 2112 10",
                        "VotingForPhotos 808 9"), "REDHERRING")
        );
    }
}

class Puzzle {

    private final List<Problem> problems;

    public Puzzle(List<Problem> problems) {
        this.problems = problems;
    }

    public static Puzzle from(List<String> problems) {
        return new Puzzle(initProblems(problems));
    }

    private static List<Problem> initProblems(List<String> problems) {
        return problems.stream()
                .map(Problem::from)
                .collect(Collectors.toList());
    }

    public String getAnswer() {
        return problems.stream()
                .sorted()
                .map(Problem::getAnswer)
                .collect(Collectors.joining());
    }
}

class Problem implements Comparable<Problem> {

    private final String content;
    private final int number;
    private final int index;

    public Problem(String content, int number, int index) {
        this.content = content;
        this.number = number;
        this.index = index;
    }

    public static Problem from(String problem) {
        String[] split = problem.split(" ");
        return new Problem(split[0], Integer.parseInt(split[1]), Integer.parseInt(split[2]));
    }

    public String getAnswer() {
        return content.substring(index - 1, index).toUpperCase();
    }

    @Override
    public int compareTo(Problem o) {
        return Integer.compare(number, o.number);
    }
}
~~~
