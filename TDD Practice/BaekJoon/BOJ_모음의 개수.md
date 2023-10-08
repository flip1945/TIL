# 모음의 개수 (Bronze 4)

### 문제 설명

영문 문장을 입력받아 모음의 개수를 세는 프로그램을 작성하시오. 모음은 'a', 'e', 'i', 'o', 'u'이며 대문자 또는 소문자이다.

출처 : https://www.acmicpc.net/problem/1264

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String input = scanner.nextLine();
        while (!input.equals("#")) {
            Sentence sentence = new Sentence(input);
            System.out.println(sentence.countVowels());
            input = scanner.nextLine();
        }
    }
}

class Sentence {

    private final String sentence;

    public Sentence(String sentence) {
        this.sentence = sentence;
    }

    public int countVowels() {
        return (int) sentence.chars()
                .mapToObj(Letter::new)
                .filter(Letter::isVowel)
                .count();
    }
}

class Letter {

    private static final Set<Character> VOWELS_SET = Set.of('a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U');

    private final char letter;

    public Letter(char letter) {
        this.letter = letter;
    }

    public Letter(int letter) {
        this((char) letter);
    }

    public boolean isVowel() {
        return VOWELS_SET.contains(letter);
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
import org.junit.jupiter.params.provider.ValueSource;

import java.util.Set;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSentence")
    @DisplayName("문장에 포함된 모음의 개수를 센다.")
    void sentenceCountsNumberOfVowels(String sentence, int expected) {
        Sentence sut = new Sentence(sentence);

        int actual = sut.countVowels();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSentence() {
        return Stream.of(
                Arguments.of("Cook", 2),
                Arguments.of("Test", 1),
                Arguments.of("AAA", 3),
                Arguments.of("How are you today?", 7),
                Arguments.of("Quite well, thank you, how about yourself?", 14),
                Arguments.of("I live at number twenty four.", 9)
        );
    }

    @ParameterizedTest
    @ValueSource(chars = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'})
    @DisplayName("문자는 주어진 문자가 모음이라면 true를 반환한다.")
    void vowelsReturnsTrueGivenVowelLetter(char letter) {
        Letter sut = new Letter(letter);

        boolean actual = sut.isVowel();

        assertTrue(actual);
    }

    @ParameterizedTest
    @ValueSource(chars = {'b', 'c', 'd', 'f'})
    @DisplayName("문자는 주어진 문자가 모음이 아니라면 false를 반환한다.")
    void vowelsReturnsFalseGivenNotVowelLetter(char letter) {
        Letter sut = new Letter(letter);

        boolean actual = sut.isVowel();

        assertFalse(actual);
    }
}

class Sentence {

    private final String sentence;

    public Sentence(String sentence) {
        this.sentence = sentence;
    }

    public int countVowels() {
        return (int) sentence.chars()
                .mapToObj(Letter::new)
                .filter(Letter::isVowel)
                .count();
    }
}

class Letter {

    private static final Set<Character> VOWELS_SET = Set.of('a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U');

    private final char letter;

    public Letter(char letter) {
        this.letter = letter;
    }

    public Letter(int letter) {
        this((char) letter);
    }

    public boolean isVowel() {
        return VOWELS_SET.contains(letter);
    }
}
~~~
