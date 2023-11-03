# Canadians, eh? (Bronze 3)

### 문제 설명

You have received a warning that foreign spies may have infiltrated the great country of Canada. Fortunately, you have developed a foolproof method to determine which potential suspects are truly Canadian: A person is a true Canadian if and only if they end every sentence exactly with eh? .

Given the transcript of a sentence spoken by a suspected spy, write a program to determine if the suspect is truly Canadian…or merely an imposter in disguise!

출처 : https://www.acmicpc.net/problem/24923

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Sentence sentence = new Sentence(scanner.nextLine());

        System.out.println(sentence.checkCanadian());
    }
}

class Sentence {

    private static final String CANADIAN = "Canadian!";
    private static final String NOT_CANADIAN = "Imposter!";
    private static final String CANADIAN_SUFFIX = "eh?";

    private final String content;

    public Sentence(String content) {
        this.content = content;
    }

    public String checkCanadian() {
        return isCanadianSentence(content) ? CANADIAN : NOT_CANADIAN;
    }

    private boolean isCanadianSentence(String sentence) {
        return sentence.endsWith(CANADIAN_SUFFIX);
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
    @DisplayName("캐나다 사람인지 아닌지 판단한다.")
    void sentenceDetermineCanadian(String sentence, String expected) {
        Sentence sut = new Sentence(sentence);

        String actual = sut.checkCanadian();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSentence() {
        return Stream.of(
                Arguments.of("The weather is nice, eh?", "Canadian!"),
                Arguments.of("I'm Canadian I swear!", "Imposter!"),
                Arguments.of("eh?", "Canadian!"),
                Arguments.of("I'm not suspicious, Eh?", "Imposter!")
        );
    }
}

class Sentence {

    private static final String CANADIAN = "Canadian!";
    private static final String NOT_CANADIAN = "Imposter!";
    private static final String CANADIAN_SUFFIX = "eh?";

    private final String content;

    public Sentence(String content) {
        this.content = content;
    }

    public String checkCanadian() {
        return isCanadianSentence(content) ? CANADIAN : NOT_CANADIAN;
    }

    private boolean isCanadianSentence(String sentence) {
        return sentence.endsWith(CANADIAN_SUFFIX);
    }
}
~~~
