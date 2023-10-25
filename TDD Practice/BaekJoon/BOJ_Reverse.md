# Reverse (Bronze 4)

### 문제 설명

In the String class, there exists a function called substring. Your task is to do the opposite of the substring function. Rather than returning a specified substring within the String, you will output the String with the substring taken out.

출처 : https://www.acmicpc.net/problem/26546

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        for (int i = 0; i < n; i++) {
            StringTokenizer st = new StringTokenizer(scanner.nextLine());

            String word = st.nextToken();
            int startIndex = Integer.parseInt(st.nextToken());
            int endIndex = Integer.parseInt(st.nextToken());

            ExcludeWord excludeWord = new ExcludeWord(word);
            System.out.println(excludeWord.substring(startIndex, endIndex));
        }
    }
}

class ExcludeWord {

    private static final int BEGIN_INDEX = 0;

    private final String word;

    public ExcludeWord(String word) {
        this.word = word;
    }

    public String substring(int startIndex, int endIndex) {
        return getLeftSubstring(startIndex) + getRightSubstring(endIndex);
    }

    private String getLeftSubstring(int startIndex) {
        return word.substring(BEGIN_INDEX, startIndex);
    }

    private String getRightSubstring(int endIndex) {
        return word.substring(endIndex);
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
    @MethodSource("provideWord")
    @DisplayName("주어진 범위를 제외한 문자열을 반환한다.")
    void excludeWordReturnsExcludeRangeSubstring(String word, int startIndex, int endIndex, String expected) {
        ExcludeWord sut = new ExcludeWord(word);

        String actual = sut.substring(startIndex, endIndex);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideWord() {
        return Stream.of(
                Arguments.of("COMPUTER", 1, 3, "CPUTER"),
                Arguments.of("SCIENCE", 3, 7, "SCI"),
                Arguments.of("RULES", 3, 4, "RULS")
        );
    }
}

class ExcludeWord {

    private static final int BEGIN_INDEX = 0;

    private final String word;

    public ExcludeWord(String word) {
        this.word = word;
    }

    public String substring(int startIndex, int endIndex) {
        return getLeftSubstring(startIndex) + getRightSubstring(endIndex);
    }

    private String getLeftSubstring(int startIndex) {
        return word.substring(BEGIN_INDEX, startIndex);
    }

    private String getRightSubstring(int endIndex) {
        return word.substring(endIndex);
    }
}
~~~
