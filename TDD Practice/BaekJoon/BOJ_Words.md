# Words (Bronze 2)

### 문제 설명

A nasty virus has infected my computer. Its effect has been to attack all my text files and reverse every word in them. Your job in this problem is to write the code to restore my text files to their original condition.

As far as the virus was concerned, a word was any sequence of characters that ended with a space or an end of line character. You will see what I mean when I tell you that the first line in one of my files was:

Get ready for the New Zealand Programming Contest.

The virus turned this into:

teG ydaer rof eht weN dnalaeZ gnimmargorP .tsetnoC

See how it included the full stop with ‘Contest’ and put it before the letters?

출처 : https://www.acmicpc.net/problem/4072

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String sentence = scanner.nextLine();

        while (!sentence.equals("#")) {
            ReversedWord reversedWord = new ReversedWord(sentence);
            System.out.println(reversedWord.reverseSentence());
            sentence = scanner.nextLine();
        }
    }
}

class ReversedWord {

    private static final String DELIMITER = " ";

    private final String sentence;

    public ReversedWord(String sentence) {
        this.sentence = sentence;
    }

    public String reverseSentence() {
        return getWords().stream()
                .map(this::getReversedWord)
                .collect(Collectors.joining(DELIMITER));
    }

    private List<String> getWords() {
        return List.of(sentence.split(DELIMITER));
    }

    private String getReversedWord(String word) {
        StringBuilder sb = new StringBuilder(word);
        return sb.reverse().toString();
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
    @MethodSource("provideSentence")
    @DisplayName("뒤집어진 단어로 이루어진 문장이 주어지면 모든 단어를 뒤집어 반환한다.")
    void reversedWordReturnsReversedSentence(String sentence, String expected) {
        ReversedWord sut = new ReversedWord(sentence);

        String actual = sut.reverseSentence();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSentence() {
        return Stream.of(
                Arguments.of("I like ice-cream.", "I ekil .maerc-eci"),
                Arguments.of("I ekil .maerc-eci", "I like ice-cream."),
                Arguments.of("Don't ever say, it's over", "t'noD reve ,yas s'ti revo"),
                Arguments.of(
                        "teG ydaer rof eht weN dnalaeZ gnimmargorP tsetnoC no .ht41",
                        "Get ready for the New Zealand Programming Contest on 14th.")
        );
    }
}

class ReversedWord {

    private static final String DELIMITER = " ";

    private final String sentence;

    public ReversedWord(String sentence) {
        this.sentence = sentence;
    }

    public String reverseSentence() {
        return getWords().stream()
                .map(this::getReversedWord)
                .collect(Collectors.joining(DELIMITER));
    }

    private List<String> getWords() {
        return List.of(sentence.split(DELIMITER));
    }

    private String getReversedWord(String word) {
        StringBuilder sb = new StringBuilder(word);
        return sb.reverse().toString();
    }
}
~~~
