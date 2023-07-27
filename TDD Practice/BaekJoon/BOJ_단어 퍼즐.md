# 단어 퍼즐 (Bronze 1)

### 문제 설명

준하는 유치원에서 단어 퍼즐게임을 즐겨한다.

단어 퍼즐게임이란, 주어진 알파벳들을 섞어서 단어를 만드는 게임이다.

천재 준하는 알파벳을 임의로 조합하여, 사전과 매칭된 단어를 만드는 프로그램을 만들어 단어를 완성시켰다.

그러나 완성된 단어를 원장님에게 가져가려는 순간, 지나가던 강민이와 부딫혀서 단어조각을 땅에 떨어뜨리고 말았다.

준하는 어찌어찌 조각을 회수했지만, 순서는 뒤죽박죽이 되었고, 알파벳이 부족하거나 다른 알파벳이 섞였을 수도 있다.

준하가 처음에 완성한 단어와 나중에 회수한 알파벳들이 주어질 때,

준하가 알파벳을 제대로 회수했는지 안했는지 판단하는 프로그램을 만들어주자.

출처 : https://www.acmicpc.net/problem/9946

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String originWord = scanner.nextLine();
        String anotherWord = scanner.nextLine();
        int i = 1;

        while (!originWord.equals("END")) {
            Anagram anagram = new Anagram(originWord, anotherWord);
            System.out.println("Case " + i + ": " + anagram.isAnagram());

            originWord = scanner.nextLine();
            anotherWord = scanner.nextLine();
            i++;
        }
    }
}

class Anagram {

    public static final String TRUE_STRING = "same";
    public static final String DIFFERENT_STRING = "different";

    private final String originWord;
    private final String anotherWord;

    public Anagram(String originWord, String anotherWord) {
        this.originWord = originWord;
        this.anotherWord = anotherWord;
    }

    public String isAnagram() {
        return getSortedString(originWord).equals(getSortedString(anotherWord)) ? TRUE_STRING : DIFFERENT_STRING;
    }

    private String getSortedString(String originWord) {
        return Arrays.stream(originWord.split(""))
                .sorted()
                .collect(Collectors.joining());
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
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideWords")
    @DisplayName("주어진 두 단어가 아나그램이라면 same 아니라면 different를 반환한다.")
    void isAnagramTest(String originWord, String anotherWord, String expected) {
        Anagram sut = new Anagram(originWord, anotherWord);

        String actual = sut.isAnagram();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideWords() {
        return Stream.of(
                Arguments.of("a", "a", "same"),
                Arguments.of("a", "b", "different"),
                Arguments.of("ab", "ba", "same"),
                Arguments.of("testing", "intestg", "same"),
                Arguments.of("abc", "aabbbcccc", "different"),
                Arguments.of("abcabcbcc", "aabbbcccc", "same"),
                Arguments.of("abc", "xyz", "different")
        );
    }
}

class Anagram {

    public static final String TRUE_STRING = "same";
    public static final String DIFFERENT_STRING = "different";

    private final String originWord;
    private final String anotherWord;

    public Anagram(String originWord, String anotherWord) {
        this.originWord = originWord;
        this.anotherWord = anotherWord;
    }

    public String isAnagram() {
        return getSortedString(originWord).equals(getSortedString(anotherWord)) ? TRUE_STRING : DIFFERENT_STRING;
    }

    private String getSortedString(String originWord) {
        return Arrays.stream(originWord.split(""))
                .sorted()
                .collect(Collectors.joining());
    }
}
~~~
