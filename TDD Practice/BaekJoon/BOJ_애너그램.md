# 애너그램 (Bronze 1)

### 문제 설명

두 단어 A와 B가 주어졌을 때, A에 속하는 알파벳의 순서를 바꾸어서 B를 만들 수 있다면, A와 B를 애너그램이라고 한다.

두 단어가 애너그램인지 아닌지 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/6996

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        AnagramChecker checker = AnagramChecker.INSTANCE;
        int numTests = sc.nextInt();
        for (int i = 0; i < numTests; i++) {
            String first = sc.next().toLowerCase();
            String second = sc.next().toLowerCase();

            System.out.println(first + " & " + second + " are " + (checker.check(first, second) ? "anagrams." : "NOT anagrams."));
        }
    }
}

enum AnagramChecker {
    INSTANCE;

    public boolean check(String first, String second) {
        if (first.length() != second.length()) {
            return false;
        }

        Map<Character, Integer> firstLetterCounter = getLetterCounter(first);
        Map<Character, Integer> secondLetterCounter = getLetterCounter(second);

        for (char ch = 'a'; ch <= 'z'; ch++) {
            if (!firstLetterCounter.get(ch).equals(secondLetterCounter.get(ch))) {
                return false;
            }
        }
        return true;
    }

    private Map<Character, Integer> getLetterCounter(String word) {
        Map<Character, Integer> letterCounter = initLetterCounter();
        for (char ch : word.toCharArray()) {
            letterCounter.put(ch, letterCounter.get(ch) + 1);
        }
        return letterCounter;
    }

    private Map<Character, Integer> initLetterCounter() {
        Map<Character, Integer> letterCounter = new HashMap<>();
        for (char ch = 'a'; ch <= 'z'; ch++) {
            letterCounter.put(ch, 0);
        }
        return letterCounter;
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

import java.util.HashMap;
import java.util.Map;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideWords")
    @DisplayName("주어진 두 단어가 애너그램인지 아닌지 확인한다.")
    void it_checks_if_the_two_words_are_anagrams(String first, String second, boolean expected) {
        AnagramChecker sut = AnagramChecker.INSTANCE;

        boolean actual = sut.check(first, second);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideWords() {
        return Stream.of(
                Arguments.of("sa", "as", true),
                Arguments.of("as", "a", false),
                Arguments.of("asdf", "zxcv", false),
                Arguments.of("blather", "reblath", true),
                Arguments.of("maryland", "landam", false),
                Arguments.of("bizarre", "brazier", true)
        );
    }
}

enum AnagramChecker {
    INSTANCE;

    public boolean check(String first, String second) {
        if (first.length() != second.length()) {
            return false;
        }

        Map<Character, Integer> firstLetterCounter = getLetterCounter(first);
        Map<Character, Integer> secondLetterCounter = getLetterCounter(second);

        for (char ch = 'a'; ch <= 'z'; ch++) {
            if (!firstLetterCounter.get(ch).equals(secondLetterCounter.get(ch))) {
                return false;
            }
        }
        return true;
    }

    private Map<Character, Integer> getLetterCounter(String word) {
        Map<Character, Integer> letterCounter = initLetterCounter();
        for (char ch : word.toCharArray()) {
            letterCounter.put(ch, letterCounter.get(ch) + 1);
        }
        return letterCounter;
    }

    private Map<Character, Integer> initLetterCounter() {
        Map<Character, Integer> letterCounter = new HashMap<>();
        for (char ch = 'a'; ch <= 'z'; ch++) {
            letterCounter.put(ch, 0);
        }
        return letterCounter;
    }
}
~~~
