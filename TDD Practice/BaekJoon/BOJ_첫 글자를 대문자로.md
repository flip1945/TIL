# 첫 글자를 대문자로 (Bronze 3)

### 문제 설명

문장을 읽은 뒤, 줄의 첫 글자를 대문자로 바꾸는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/4458

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        FirstLetterCapitalizer firstLetterCapitalizer = new FirstLetterCapitalizer();
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            String word = scanner.nextLine();
            System.out.println(firstLetterCapitalizer.capitalize(word));
        }
    }
}

class FirstLetterCapitalizer {

    private static final int FIRST_LETTER_INDEX = 0;
    private static final int REMAINING_WORD_BEGIN_INDEX = 1;

    public String capitalize(String word) {
        return capitalizeFirstLetter(word) + getRemainingString(word);
    }

    private String capitalizeFirstLetter(String word) {
        return word.substring(FIRST_LETTER_INDEX, REMAINING_WORD_BEGIN_INDEX).toUpperCase();
    }

    private String getRemainingString(String word) {
        return word.substring(REMAINING_WORD_BEGIN_INDEX);
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

import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideWords")
    @DisplayName("문장의 첫글자를 대문자로 바꿔야 한다.")
    void shouldCapitalizeFirstLetter(String word, String expected) {
        FirstLetterCapitalizer sut = new FirstLetterCapitalizer();

        String actual = sut.capitalize(word);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideWords() {
        return Stream.of(
                Arguments.of("a", "A"),
                Arguments.of("b", "B"),
                Arguments.of("a ", "A "),
                Arguments.of("abc", "Abc"),
                Arguments.of("a bc", "A bc"),
                Arguments.of("A bc", "A bc"),
                Arguments.of("powdered Toast Man", "Powdered Toast Man"),
                Arguments.of("skeletor", "Skeletor"),
                Arguments.of("Electra Woman and Dyna Girl", "Electra Woman and Dyna Girl"),
                Arguments.of("she-Ra Princess of Power", "She-Ra Princess of Power"),
                Arguments.of("darth Vader", "Darth Vader")
        );
    }
}

class FirstLetterCapitalizer {

    private static final int FIRST_LETTER_INDEX = 0;
    private static final int REMAINING_WORD_BEGIN_INDEX = 1;

    public String capitalize(String word) {
        return capitalizeFirstLetter(word) + getRemainingString(word);
    }

    private String capitalizeFirstLetter(String word) {
        return word.substring(FIRST_LETTER_INDEX, REMAINING_WORD_BEGIN_INDEX).toUpperCase();
    }

    private String getRemainingString(String word) {
        return word.substring(REMAINING_WORD_BEGIN_INDEX);
    }
}
~~~
