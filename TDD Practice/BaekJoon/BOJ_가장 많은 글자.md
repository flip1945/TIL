# 가장 많은 글자 (Bronze 2)

### 문제 설명

영어에서는 어떤 글자가 다른 글자보다 많이 쓰인다. 예를 들어, 긴 글에서 약 12.31% 글자는 e이다.

어떤 글이 주어졌을 때, 가장 많이 나온 글자를 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1371

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Sentences sentences = new Sentences(inputSentences(scanner));
        System.out.println(sentences.mostFrequencyLetter());
    }

    private static List<String> inputSentences(Scanner scanner) {
        List<String> sentences = new ArrayList<>();
        while (scanner.hasNextLine()) {
            sentences.add(scanner.nextLine());
        }
        return sentences;
    }
}

class Sentences {

    private final LetterFrequency letterFrequency;

    public Sentences(List<String> sentences) {
        this.letterFrequency = LetterFrequency.from(sentences);
    }

    public String mostFrequencyLetter() {
        int mostFrequencyCount = mostFrequencyCount();
        int[] frequency = letterFrequency.getFrequency();
        return mostFrequencyLetters(mostFrequencyCount, frequency);
    }

    private String mostFrequencyLetters(int mostFrequencyCount, int[] frequency) {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < frequency.length; i++) {
            if (frequency[i] == mostFrequencyCount) {
                result.append(Letter.toLetter(i));
            }
        }
        return String.valueOf(result);
    }

    private int mostFrequencyCount() {
        return Arrays.stream(letterFrequency.getFrequency())
                .max()
                .orElse(Integer.MIN_VALUE);
    }
}

class LetterFrequency {

    private final int[] frequency;

    public LetterFrequency(int[] frequency) {
        this.frequency = frequency;
    }

    public static LetterFrequency from(List<String> sentences) {
        return new LetterFrequency(initFrequency(sentences));
    }

    private static int[] initFrequency(List<String> sentences) {
        int[] result = new int[26];
        for (String sentence : sentences) {
            addFrequencyFromSentence(result, sentence);
        }
        return result;
    }

    private static void addFrequencyFromSentence(int[] result, String sentence) {
        for (char letter : sentence.toCharArray()) {
            addFrequency(result, letter);
        }
    }

    private static void addFrequency(int[] result, char letter) {
        if (Character.isWhitespace(letter)) {
            return;
        }
        result[Letter.toLetterSequence(letter)]++;
    }

    public int[] getFrequency() {
        return frequency;
    }
}

class Letter {

    private static final char START_LETTER = 'a' ;

    private Letter() {

    }

    public static char toLetter(int sequence) {
        return (char)(sequence + START_LETTER);
    }

    public static int toLetterSequence(char letter) {
        return letter - START_LETTER;
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

import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSentences")
    @DisplayName("문장들이 주어지면 가장 많이 나온 글자를 반환한다.")
    void sentencesReturnsMostFrequencyLetter(List<String> sentences, String expected) {
        Sentences sut = new Sentences(sentences);

        String actual = sut.mostFrequencyLetter();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSentences() {
        return Stream.of(
                Arguments.of(List.of("aab"), "a"),
                Arguments.of(List.of("aabbb"), "b"),
                Arguments.of(List.of("aabbb", "cccc"), "c"),
                Arguments.of(List.of("aabbb", "cccc", "     "), "c"),
                Arguments.of(List.of("aabbb", "ccc"), "bc"),
                Arguments.of(List.of("baekjoon online judge"), "eno")
        );
    }
}

class Sentences {

    private final LetterFrequency letterFrequency;

    public Sentences(List<String> sentences) {
        this.letterFrequency = LetterFrequency.from(sentences);
    }

    public String mostFrequencyLetter() {
        int mostFrequencyCount = mostFrequencyCount();
        int[] frequency = letterFrequency.getFrequency();
        return mostFrequencyLetters(mostFrequencyCount, frequency);
    }

    private String mostFrequencyLetters(int mostFrequencyCount, int[] frequency) {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < frequency.length; i++) {
            if (frequency[i] == mostFrequencyCount) {
                result.append(Letter.toLetter(i));
            }
        }
        return String.valueOf(result);
    }

    private int mostFrequencyCount() {
        return Arrays.stream(letterFrequency.getFrequency())
                .max()
                .orElse(Integer.MIN_VALUE);
    }
}

class LetterFrequency {

    private final int[] frequency;

    public LetterFrequency(int[] frequency) {
        this.frequency = frequency;
    }

    public static LetterFrequency from(List<String> sentences) {
        return new LetterFrequency(initFrequency(sentences));
    }

    private static int[] initFrequency(List<String> sentences) {
        int[] result = new int[26];
        for (String sentence : sentences) {
            addFrequencyFromSentence(result, sentence);
        }
        return result;
    }

    private static void addFrequencyFromSentence(int[] result, String sentence) {
        for (char letter : sentence.toCharArray()) {
            addFrequency(result, letter);
        }
    }

    private static void addFrequency(int[] result, char letter) {
        if (Character.isWhitespace(letter)) {
            return;
        }
        result[Letter.toLetterSequence(letter)]++;
    }

    public int[] getFrequency() {
        return frequency;
    }
}

class Letter {

    private static final char START_LETTER = 'a' ;

    private Letter() {

    }

    public static char toLetter(int sequence) {
        return (char)(sequence + START_LETTER);
    }

    public static int toLetterSequence(char letter) {
        return letter - START_LETTER;
    }
}
~~~
