# 애너그램 (Bronze 1)

### 문제 설명

두 단어 A와 B가 주어졌을 때, A에 속하는 알파벳의 순서를 바꾸어서 B를 만들 수 있다면, A와 B를 애너그램이라고 한다.

두 단어가 애너그램인지 아닌지 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/6996

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

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
        return sortedString(first).equals(sortedString(second));
    }

    private String sortedString(String word) {
        return Arrays.stream(word.split(""))
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
import java.util.stream.Collectors;
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
        return sortedString(first).equals(sortedString(second));
    }

    private String sortedString(String word) {
        return Arrays.stream(word.split(""))
                .sorted()
                .collect(Collectors.joining());
    }
}
~~~
