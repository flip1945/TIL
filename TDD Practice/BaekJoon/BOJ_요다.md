# 요다 (Bronze 2)

### 문제 설명

어린 제다이들은 요다와 대화하는 법을 배워야 한다. 요다는 모든 문장에서 가장 앞 단어 두 개를 제일 마지막에 말한다.

어떤 문장이 주어졌을 때, 요다의 말로 바꾸는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/5363

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            System.out.println(new YodaWord(scanner.nextLine()));
        }
    }
}

class YodaWord {

    public static final String DELIMITER = " ";
    public static final int YODA_WORD_SHIFT = -2;
    private final List<String> words;

    public YodaWord(String sentence) {
        this.words = Arrays.asList(sentence.split(DELIMITER));
    }

    @Override
    public String toString() {
        return String.join(DELIMITER, shiftToYodaWord());
    }

    private List<String> shiftToYodaWord() {
        List<String> result = new ArrayList<>(words);
        Collections.rotate(result, YODA_WORD_SHIFT);
        return result;
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
    @MethodSource("provideSentences")
    @DisplayName("가장 앞 두 단어를 제일 마지막에 두고 반환한다.")
    void yodaWordTest(String sentence, String expected) {
        YodaWord sut = new YodaWord(sentence);

        String actual = sut.toString();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSentences() {
        return Stream.of(
                Arguments.of("a b c", "c a b"),
                Arguments.of("a b c d", "c d a b"),
                Arguments.of("I will go now to find the Wookiee", "go now to find the Wookiee I will"),
                Arguments.of("Solo found the death star near planet Kessel", "the death star near planet Kessel Solo found"),
                Arguments.of("I'll fight Darth Maul here and now", "Darth Maul here and now I'll fight"),
                Arguments.of("Vader will find Luke before he can escape", "find Luke before he can escape Vader will")
        );
    }
}

class YodaWord {

    public static final String DELIMITER = " ";
    public static final int YODA_WORD_SHIFT = -2;
    private final List<String> words;

    public YodaWord(String sentence) {
        this.words = Arrays.asList(sentence.split(DELIMITER));
    }

    @Override
    public String toString() {
        return String.join(DELIMITER, shiftToYodaWord());
    }

    private List<String> shiftToYodaWord() {
        List<String> result = new ArrayList<>(words);
        Collections.rotate(result, YODA_WORD_SHIFT);
        return result;
    }
}
~~~
