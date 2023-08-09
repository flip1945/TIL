# Serious Problem (Bronze 3)

### 문제 설명

2와 e는 발음이 비슷해, 둘을 섞어서 말하면 듣는 사람을 짜증나게 만들 수 있다.

지민이는 이 점을 이용해 은수를 미치게 하고 있다. 은수를 위해 지민이가 말한 문자열 s가 주어질때, 2의 등장 횟수가 더 많은지, e의 등장 횟수가 더 많은지 도와주자.

출처 : https://www.acmicpc.net/problem/17094

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();

        SeriousPronunciation pronunciation = new SeriousPronunciation(scanner.nextLine());
        System.out.println(pronunciation.checkPronunciation());
    }
}

class SeriousPronunciation {

    public static final char TWO_PRONUNCIATION = '2';
    public static final char E_PRONUNCIATION = 'e';
    public static final String TWO_STRING = "2";
    public static final String E_STRING = "e";
    public static final String YEE_STRING = "yee";

    private final String pronunciation;

    public SeriousPronunciation(String pronunciation) {
        this.pronunciation = pronunciation;
    }

    public String checkPronunciation() {
        return check(getCountOf(TWO_PRONUNCIATION), getCountOf(E_PRONUNCIATION));
    }

    private String check(int countOfTwo, int countOfE) {
        if (countOfE == countOfTwo) {
            return YEE_STRING;
        }
        return countOfTwo > countOfE ? TWO_STRING : E_STRING;
    }

    private int getCountOf(char singlePronunciation) {
        return (int) pronunciation.chars()
                .filter(ch -> ch == singlePronunciation)
                .count();
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
    @MethodSource("providePronunciations")
    @DisplayName("2가 더 많다면 2, e가 더 많다면 e, 같다면 yee를 반환한다.")
    void testCheckPronunciation(String pronunciation, String expected) {
        SeriousPronunciation sut = new SeriousPronunciation(pronunciation);

        String actual = sut.checkPronunciation();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> providePronunciations() {
        return Stream.of(
                Arguments.of("2", "2"),
                Arguments.of("e", "e"),
                Arguments.of("2e", "yee"),
                Arguments.of("22e", "2"),
                Arguments.of("2e2e", "yee"),
                Arguments.of("2e2ee", "e"),
                Arguments.of("222eee222eee", "yee"),
                Arguments.of("e2e", "e")
        );
    }
}

class SeriousPronunciation {

    public static final char TWO_PRONUNCIATION = '2';
    public static final char E_PRONUNCIATION = 'e';
    public static final String TWO_STRING = "2";
    public static final String E_STRING = "e";
    public static final String YEE_STRING = "yee";

    private final String pronunciation;

    public SeriousPronunciation(String pronunciation) {
        this.pronunciation = pronunciation;
    }

    public String checkPronunciation() {
        return check(getCountOf(TWO_PRONUNCIATION), getCountOf(E_PRONUNCIATION));
    }

    private String check(int countOfTwo, int countOfE) {
        if (countOfE == countOfTwo) {
            return YEE_STRING;
        }
        return countOfTwo > countOfE ? TWO_STRING : E_STRING;
    }

    private int getCountOf(char singlePronunciation) {
        return (int) pronunciation.chars()
                .filter(ch -> ch == singlePronunciation)
                .count();
    }
}
~~~
