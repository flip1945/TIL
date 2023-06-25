# 가장 쉬운 문제를 찾는 문제 (Bronze 2)

### 문제 설명

예선 문제를 성실하게 복습한 학생들이라면 예선에 출제된 5문제가 난이도 순서대로 정렬되어 있다는 것을 알아차렸을 것이다.

하지만 본선은 문제 제목에 대해 사전순으로 정렬했기 때문에 난이도 순서대로 정렬되어 있지 않을 수 있다.

문제 제목과 문제의 난이도가 주어지면 가장 쉬운 문제의 제목을 출력하는 프로그램을 작성하자.

문제의 난이도는 자연수로 표현되며, 수가 클수록 어려운 문제다.

출처 : https://www.acmicpc.net/problem/22966

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> quizAndDifficulty = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            quizAndDifficulty.add(scanner.nextLine());
        }

        Quizzes quizzes = Quizzes.from(quizAndDifficulty);
        System.out.println(quizzes.getEasiestQuizName());
    }
}

class Quizzes {

    private final List<Quiz> quizzes;

    private Quizzes(List<Quiz> quizzes) {
        this.quizzes = quizzes;
    }

    public static Quizzes from(List<String> quizNameAndDifficulties) {
        return new Quizzes(convertToQuizzes(quizNameAndDifficulties));
    }

    private static List<Quiz> convertToQuizzes(List<String> quizNameAndDifficulties) {
        return quizNameAndDifficulties.stream()
                .map(Quiz::from)
                .collect(Collectors.toList());
    }

    public String getEasiestQuizName() {
        return quizzes.stream()
                .sorted()
                .map(Quiz::getName)
                .findFirst()
                .orElse("");
    }
}

class Quiz implements Comparable<Quiz> {

    private final String name;
    private final int difficulty;

    public static Quiz from(String quizAndDifficulty) {
        StringTokenizer stringTokenizer = new StringTokenizer(quizAndDifficulty);
        return new Quiz(stringTokenizer.nextToken(), Integer.parseInt(stringTokenizer.nextToken()));
    }

    private Quiz(String name, int difficulty) {
        this.name = name;
        this.difficulty = difficulty;
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(Quiz o) {
        int result = Integer.compare(this.difficulty, o.difficulty);
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

import java.util.*;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideQuizNameAndDifficulties")
    @DisplayName("가장 쉬운 문제의 이름을 반환한다.")
    void getEasiestQuizTest(List<String> quizNameAndDifficulties, String expected) {
        Quizzes sut = Quizzes.from(quizNameAndDifficulties);

        String actual = sut.getEasiestQuizName();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideQuizNameAndDifficulties() {
        return Stream.of(
                Arguments.of(List.of("A 1"), "A"),
                Arguments.of(List.of("B 1"), "B"),
                Arguments.of(List.of("B 3", "A 2"), "A"),
                Arguments.of(List.of("B 3", "A 2", "C 1"), "C"),
                Arguments.of(List.of("ABCDE 4", "BCDEF 2", "CDEFG 3"), "BCDEF")
        );
    }
}

class Quizzes {

    private final List<Quiz> quizzes;

    private Quizzes(List<Quiz> quizzes) {
        this.quizzes = quizzes;
    }

    public static Quizzes from(List<String> quizNameAndDifficulties) {
        return new Quizzes(convertToQuizzes(quizNameAndDifficulties));
    }

    private static List<Quiz> convertToQuizzes(List<String> quizNameAndDifficulties) {
        return quizNameAndDifficulties.stream()
                .map(Quiz::from)
                .collect(Collectors.toList());
    }

    public String getEasiestQuizName() {
        return quizzes.stream()
                .sorted()
                .map(Quiz::getName)
                .findFirst()
                .orElse("");
    }
}

class Quiz implements Comparable<Quiz> {

    private final String name;
    private final int difficulty;

    public static Quiz from(String quizAndDifficulty) {
        StringTokenizer stringTokenizer = new StringTokenizer(quizAndDifficulty);
        return new Quiz(stringTokenizer.nextToken(), Integer.parseInt(stringTokenizer.nextToken()));
    }

    private Quiz(String name, int difficulty) {
        this.name = name;
        this.difficulty = difficulty;
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(Quiz o) {
        int result = Integer.compare(this.difficulty, o.difficulty);
        if (result != 0) {
            return result;
        }
        return this.name.compareTo(o.name);
    }
}
~~~
