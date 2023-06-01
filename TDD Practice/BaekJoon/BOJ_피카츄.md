# 피카츄 (Bronze 1)

### 문제 설명

피카츄는 "pi", "ka", "chu"를 발음할 수 있다. 따라서, 피카츄는 이 세 음절을 합친 단어만 발음할 수 있다. 예를 들면, "pikapi"와 "pikachu"가 있다.

문자열 S가 주어졌을 때, 피카츄가 발음할 수 있는 문자열인지 아닌지 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/14405

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(PikachuStringChecker.check(scanner.nextLine()));
    }
}

class PikachuStringChecker {

    public static final int IS_ALL_REMOVED_PIKACHU_WORD = 0;
    public static final String TRUE_STRING = "YES";
    public static final String FALSE_STRING = "NO";

    public static String check(String word) {
        return removePikachuWord(word).length() == IS_ALL_REMOVED_PIKACHU_WORD ? TRUE_STRING : FALSE_STRING;
    }

    private static String removePikachuWord(String word) {
        return word.replaceAll("pi|ka|chu", "");
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

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideWords")
    @DisplayName("pi ka chu로만 만들 수 있는 문자면 YES 아니면 NO를 반환한다.")
    void isPikachuStringTest(String word, String expected) {
        String actual = PikachuStringChecker.check(word);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideWords() {
        return Stream.of(
                Arguments.of("pi", "YES"),
                Arguments.of("pii", "NO"),
                Arguments.of("ka", "YES"),
                Arguments.of("chu", "YES"),
                Arguments.of("pika", "YES"),
                Arguments.of("p", "NO"),
                Arguments.of("a", "NO"),
                Arguments.of("pik", "NO"),
                Arguments.of("pikach", "NO"),
                Arguments.of("pikapi", "YES"),
                Arguments.of("pikaqiu", "NO"),
                Arguments.of("piika", "NO"),
                Arguments.of("chupikachupipichu", "YES"),
                Arguments.of("kpia", "NO")
        );
    }
}

class PikachuStringChecker {

    public static final int IS_ALL_REMOVED_PIKACHU_WORD = 0;
    public static final String TRUE_STRING = "YES";
    public static final String FALSE_STRING = "NO";

    public static String check(String word) {
        return removePikachuWord(word).length() == IS_ALL_REMOVED_PIKACHU_WORD ? TRUE_STRING : FALSE_STRING;
    }

    private static String removePikachuWord(String word) {
        return word.replaceAll("pi|ka|chu", "");
    }
}
~~~
