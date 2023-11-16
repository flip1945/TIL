# Reverse Words (Large) (Bronze 2)

### 문제 설명

Given a list of space separated words, reverse the order of the words. Each line of text contains L letters and W words. A line will only consist of letters and space characters. There will be exactly one space character between each pair of consecutive words.

출처 : https://www.acmicpc.net/problem/12606

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 1; i <= n; i++) {
            String sentence = scanner.nextLine();
            ReversedSentence reversedSentence = new ReversedSentence(sentence);
            System.out.println("Case #" + i + ": " + reversedSentence.reverse());
        }
    }
}

class ReversedSentence {

    private static final String DELIMITER = " ";

    private final String sentence;

    public ReversedSentence(String sentence) {
        this.sentence = sentence;
    }

    public String reverse() {
        return String.join(DELIMITER, getReversedWords());
    }

    private List<String> getReversedWords() {
        List<String> words = getWords();

        List<String> result = new ArrayList<>();
        for (int i = words.size() - 1; i >= 0; i--) {
            result.add(words.get(i));
        }
        return result;
    }

    private List<String> getWords() {
        return List.of(sentence.split(DELIMITER));
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
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSentence")
    @DisplayName("단어의 순서를 뒤집은 문장을 반환한다.")
    void reversedSentenceReverseWordsOrder(String sentence, String expected) {
        ReversedSentence sut = new ReversedSentence(sentence);

        String actual = sut.reverse();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSentence() {
        return Stream.of(
                Arguments.of("I like ice-cream", "ice-cream like I"),
                Arguments.of("this is a test", "test a is this"),
                Arguments.of("foobar", "foobar"),
                Arguments.of("all your base", "base your all")
        );
    }
}

class ReversedSentence {

    private static final String DELIMITER = " ";

    private final String sentence;

    public ReversedSentence(String sentence) {
        this.sentence = sentence;
    }

    public String reverse() {
        return String.join(DELIMITER, getReversedWords());
    }

    private List<String> getReversedWords() {
        List<String> words = getWords();

        List<String> result = new ArrayList<>();
        for (int i = words.size() - 1; i >= 0; i--) {
            result.add(words.get(i));
        }
        return result;
    }

    private List<String> getWords() {
        return List.of(sentence.split(DELIMITER));
    }
}
~~~
