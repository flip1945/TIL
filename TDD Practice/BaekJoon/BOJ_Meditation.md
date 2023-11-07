# Meditation (Silver 5)

### 문제 설명

Luna had a stressful day and she wants to do a meditation routine that relaxes her well. Luna’s routines are more or less relaxing and to determine how relaxing a routine is, Luna computes its score: the higher the score, the more relaxing it is!

Luna has graded each of the n exercises with a positive integer and the score of a routine is simply the sum of the grades of its individual exercises. She gives you her list of graded exercises and asks you what is the maximal grade of a routine composed of k different exercises.

출처 : https://www.acmicpc.net/problem/21194

---

#### 풀이
~~~java
import java.util.Comparator;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int k = scanner.nextInt();
        List<Integer> exerciseScores = inputExerciseScores(scanner, n);

        Meditation meditation = new Meditation(exerciseScores, k);
        System.out.println(meditation.maxScore());
    }

    private static List<Integer> inputExerciseScores(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class Meditation {

    private final List<Integer> exerciseScores;
    private final int scoreSize;

    public Meditation(List<Integer> exerciseScores, int scoreSize) {
        this.exerciseScores = exerciseScores;
        this.scoreSize = scoreSize;
    }

    public int maxScore() {
        return exerciseScores.stream()
                .sorted(Comparator.reverseOrder())
                .limit(scoreSize)
                .mapToInt(Integer::intValue)
                .sum();
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

import java.util.Comparator;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideExerciseScores")
    @DisplayName("명상은 운동 루틴의 가장 높은 점수를 계산한다.")
    void meditationComputesMaxScoreExerciseRoutine(List<Integer> exerciseScores, int scoreSize, int expected) {
        Meditation sut = new Meditation(exerciseScores, scoreSize);

        int actual = sut.maxScore();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideExerciseScores() {
        return Stream.of(
                Arguments.of(List.of(3, 2, 1), 1, 3),
                Arguments.of(List.of(4, 3, 2, 1), 1, 4),
                Arguments.of(List.of(1, 2, 3, 4), 1, 4),
                Arguments.of(List.of(1, 2, 3, 4), 2, 7),
                Arguments.of(List.of(10, 22, 7, 3, 10), 3, 42)
        );
    }
}

class Meditation {

    private final List<Integer> exerciseScores;
    private final int scoreSize;

    public Meditation(List<Integer> exerciseScores, int scoreSize) {
        this.exerciseScores = exerciseScores;
        this.scoreSize = scoreSize;
    }

    public int maxScore() {
        return exerciseScores.stream()
                .sorted(Comparator.reverseOrder())
                .limit(scoreSize)
                .mapToInt(Integer::intValue)
                .sum();
    }
}
~~~
