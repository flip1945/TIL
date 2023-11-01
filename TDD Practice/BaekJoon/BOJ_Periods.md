# Periods (Bronze 3)

### 문제 설명

Eric gets distracted so sometimes he forgets to put periods at the end of his sentences. To help him out, you are to put a period at the end of his sentences if the period is not already present.

출처 : https://www.acmicpc.net/problem/26560

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            Sentence sentence = new Sentence(scanner.nextLine());
            System.out.println(sentence.makePerfect());
        }
    }
}

class Sentence {

    private static final String PERIOD = ".";

    private final String content;

    public Sentence(String content) {
        this.content = content;
    }

    public String makePerfect() {
        return isPerfectSentence() ? content : getPerfectSentence();
    }

    private boolean isPerfectSentence() {
        return content.endsWith(PERIOD);
    }

    private String getPerfectSentence() {
        return content + PERIOD;
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

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSentence")
    @DisplayName("마침표가 없는 문장은 완성시킨다.")
    void sentenceMakePerfectSentenceWithPeriod(String sentence, String expected) {
        Sentence sut = new Sentence(sentence);

        String actual = sut.makePerfect();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSentence() {
        return Stream.of(
                Arguments.of("I am a man", "I am a man."),
                Arguments.of("You kicked my dog", "You kicked my dog."),
                Arguments.of("No I did not.", "No I did not."),
                Arguments.of("It was the kid that did", "It was the kid that did.")
        );
    }
}

class Sentence {

    private static final String PERIOD = ".";

    private final String content;

    public Sentence(String content) {
        this.content = content;
    }

    public String makePerfect() {
        return isPerfectSentence() ? content : getPerfectSentence();
    }

    private boolean isPerfectSentence() {
        return content.endsWith(PERIOD);
    }

    private String getPerfectSentence() {
        return content + PERIOD;
    }
}
~~~
