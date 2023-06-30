# 알파벳 전부 쓰기 (Bronze 2)

### 문제 설명

팬그램은 26개의 알파벳, a~z를 최소 한번씩 모두 사용한 문장을 말한다. 아마 가장 유명한 문장은 이것일 것이다. "The quick brown fox jumps over the lazy dog."

꿍은 다른 문장들중에 팬그램인 것은 없는지 궁금해졌다. 그래서 여러분이 할 일은 꿍을 위해 어떠한 문장이 팬그램인지 아닌지를 판별해주는 프로그램을 짜는 것이다.

팬그램에서는 알파벳의 대소문자를 구분하지 않는다고 하자.

출처 : https://www.acmicpc.net/problem/11091

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            Pangram pangram = new Pangram(scanner.nextLine());
            System.out.println(pangram.isPangram());
        }
    }
}

class Pangram {

    public static final String PANGRAM_WORD = "pangram";
    public static final String MISSING_SUFFIX = "missing ";
    public static final String DELIMITER = "";
    public static final int PANGRAM_SIZE = 26;
    public static final int START_INDEX = 0;
    public static final int END_INDEX = 26;
    private final Set<Alphabet> alphabets;

    public Pangram(String sentence) {
        this.alphabets = getAlphabets(sentence);
    }

    private Set<Alphabet> getAlphabets(String sentence) {
        return sentence.chars()
                .mapToObj(letter -> new Alphabet((char) letter))
                .filter(Alphabet::isAlphabet)
                .collect(Collectors.toSet());
    }

    public String isPangram() {
        if (alphabets.size() == PANGRAM_SIZE) {
            return PANGRAM_WORD;
        }
        return MISSING_SUFFIX + String.join(DELIMITER, getMissing(alphabets));
    }

    private List<String> getMissing(Set<Alphabet> alphabets) {
        return IntStream.range(START_INDEX, END_INDEX)
                .mapToObj(Alphabet::new)
                .filter(alphabet -> !alphabets.contains(alphabet))
                .map(Alphabet::toString)
                .collect(Collectors.toList());
    }
}

class Alphabet {

    public static final int LOWERCASE_ALPHABET_OFFSET = 97;
    private final char alphabet;

    public Alphabet(char alphabet) {
        this.alphabet = Character.toLowerCase(alphabet);
    }

    public Alphabet(int index) {
        this.alphabet = (char) (index + LOWERCASE_ALPHABET_OFFSET);
    }

    public boolean isAlphabet() {
        return Character.isAlphabetic(alphabet);
    }

    @Override
    public String toString() {
        return String.valueOf(alphabet);
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Alphabet alphabet1 = (Alphabet) o;
        return alphabet == alphabet1.alphabet;
    }

    @Override
    public int hashCode() {
        return Objects.hash(alphabet);
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
    @MethodSource("providePangramSentences")
    @DisplayName("대소문자를 구분하지 않고 모든 알파벳이 있다면 pangram을 반환한다.")
    void isPangramTest(String sentence) {
        Pangram sut = new Pangram(sentence);

        String actual = sut.isPangram();

        assertEquals("pangram", actual);
    }

    private static Stream<Arguments> providePangramSentences() {
        return Stream.of(
                Arguments.of("defghijklmnopqrstuvwxyzabc"),
                Arguments.of("defghijklmnopqrstuvwxyz abc"),
                Arguments.of("abcdefghijklmnopqrstuvwxyz"),
                Arguments.of("The quick brown fox jumps over the lazy dog.")
        );
    }

    @ParameterizedTest
    @MethodSource("provideNotPangramSentences")
    @DisplayName("대소문자를 구분하지 않고 모든 알파벳이 없다면 missing과 함께 나타나지 않은 알파벳을 정렬해 반환한다.")
    void isNotPangramTest(String sentence, String expected) {
        Pangram sut = new Pangram(sentence);

        String actual = sut.isPangram();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNotPangramSentences() {
        return Stream.of(
                Arguments.of("a", "missing bcdefghijklmnopqrstuvwxyz"),
                Arguments.of("ab", "missing cdefghijklmnopqrstuvwxyz"),
                Arguments.of("a c e", "missing bdfghijklmnopqrstuvwxyz"),
                Arguments.of("a !", "missing bcdefghijklmnopqrstuvwxyz"),
                Arguments.of("a B", "missing cdefghijklmnopqrstuvwxyz"),
                Arguments.of(".,?!'\" 92384 abcde FGHIJ", "missing klmnopqrstuvwxyz")
        );
    }
}

class Pangram {

    public static final String PANGRAM_WORD = "pangram";
    public static final String MISSING_SUFFIX = "missing ";
    public static final String DELIMITER = "";
    public static final int PANGRAM_SIZE = 26;
    public static final int START_INDEX = 0;
    public static final int END_INDEX = 26;
    private final Set<Alphabet> alphabets;

    public Pangram(String sentence) {
        this.alphabets = getAlphabets(sentence);
    }

    private Set<Alphabet> getAlphabets(String sentence) {
        return sentence.chars()
                .mapToObj(letter -> new Alphabet((char) letter))
                .filter(Alphabet::isAlphabet)
                .collect(Collectors.toSet());
    }

    public String isPangram() {
        if (alphabets.size() == PANGRAM_SIZE) {
            return PANGRAM_WORD;
        }
        return MISSING_SUFFIX + String.join(DELIMITER, getMissing(alphabets));
    }

    private List<String> getMissing(Set<Alphabet> alphabets) {
        return IntStream.range(START_INDEX, END_INDEX)
                .mapToObj(Alphabet::new)
                .filter(alphabet -> !alphabets.contains(alphabet))
                .map(Alphabet::toString)
                .collect(Collectors.toList());
    }
}

class Alphabet {

    public static final int LOWERCASE_ALPHABET_OFFSET = 97;
    private final char alphabet;

    public Alphabet(char alphabet) {
        this.alphabet = Character.toLowerCase(alphabet);
    }

    public Alphabet(int index) {
        this.alphabet = (char) (index + LOWERCASE_ALPHABET_OFFSET);
    }

    public boolean isAlphabet() {
        return Character.isAlphabetic(alphabet);
    }

    @Override
    public String toString() {
        return String.valueOf(alphabet);
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Alphabet alphabet1 = (Alphabet) o;
        return alphabet == alphabet1.alphabet;
    }

    @Override
    public int hashCode() {
        return Objects.hash(alphabet);
    }
}
~~~
