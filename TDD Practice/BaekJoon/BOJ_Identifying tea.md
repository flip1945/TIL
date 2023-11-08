# Identifying tea (Bronze 4)

### 문제 설명

Blind tea tasting is the skill of identifying a tea by using only your senses of smell and taste.

As part of the Ideal Challenge of Pure-Tea Consumers (ICPC), a local TV show is organized. During the show, a full teapot is prepared and five contestants are handed a cup of tea each. The participants must smell, taste and assess the sample so as to identify the tea type, which can be: (1) white tea; (2) green tea; (3) black tea; or (4) herbal tea. At the end, the answers are checked to determine the number of correct guesses.

Given the actual tea type and the answers provided, determine the number of contestants who got the correct answer.

출처 : https://www.acmicpc.net/problem/11549

---

#### 풀이
~~~java
import java.util.List;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int correctAnswer = scanner.nextInt();
        List<Integer> answer = List.of(scanner.nextInt(), scanner.nextInt(), scanner.nextInt(), scanner.nextInt(),
                scanner.nextInt());

        BlindTasting blindTasting = new BlindTasting(answer, correctAnswer);
        System.out.println(blindTasting.countOfCorrectAnswers());
    }
}

class BlindTasting {

    private final List<Integer> answers;
    private final int correctAnswer;

    public BlindTasting(List<Integer> answers, int correctAnswer) {
        this.answers = answers;
        this.correctAnswer = correctAnswer;
    }

    public int countOfCorrectAnswers() {
        return (int) answers.stream()
                .filter(answer -> answer == correctAnswer)
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
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideAnswers")
    @DisplayName("블라인드 테스트는 정답자의 숫자를 반환한다.")
    void blindTastingReturnsCountOfCorrectAnswer(List<Integer> answers, int correctAnswer, int expected) {
        BlindTasting sut = new BlindTasting(answers, correctAnswer);
        
        int actual = sut.countOfCorrectAnswers();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideAnswers() {
        return Stream.of(
                Arguments.of(List.of(1, 2, 3, 2, 1), 1, 2),
                Arguments.of(List.of(1, 1, 1, 1, 1), 1, 5),
                Arguments.of(List.of(4, 1, 1, 2, 1), 3, 0)
        );
    }
}

class BlindTasting {

    private final List<Integer> answers;
    private final int correctAnswer;

    public BlindTasting(List<Integer> answers, int correctAnswer) {
        this.answers = answers;
        this.correctAnswer = correctAnswer;
    }

    public int countOfCorrectAnswers() {
        return (int) answers.stream()
                .filter(answer -> answer == correctAnswer)
                .count();
    }
}
~~~
