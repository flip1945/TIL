# 무한 문자열 (Silver 5)

### 문제 설명

문자열 s가 있을 때, f(s)는 s를 무한번 붙인 문자열로 정의한다. 예를 들어, s = "abc" 인 경우에 f(s) = "abcabcabcabc..."가 된다.

다른 문자열 s와 t가 있을 때, f(s)와 f(t)가 같은 문자열인 경우가 있다. 예를 들어서, s = "abc", t = "abcabc"인 경우에 f(s)와 f(t)는 같은 문자열을 만든다.

s와 t가 주어졌을 때, f(s)와 f(t)가 같은 문자열을 만드는지 아닌지 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/12871

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        InfinityStringChecker checker = InfinityStringChecker.INSTANCE;
        System.out.println(checker.check(scanner.nextLine(), scanner.nextLine()));
    }
}

enum InfinityStringChecker {
    INSTANCE;

    public int check(String str1, String str2) {
        return isSame(str1, str2) ? 1 : 0;
    }

    private boolean isSame(String str1, String str2) {
        return str1.repeat(str2.length()).equals(str2.repeat(str1.length()));
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
    @MethodSource("provideNames")
    @DisplayName("같은 무한 문자열인지 확인한다.")
    void isSameInfinityStringTest(String str1, String str2, int expected) {
        InfinityStringChecker sut = InfinityStringChecker.INSTANCE;

        int actual = sut.check(str1, str2);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNames() {
        return Stream.of(
                Arguments.of("ab", "abab", 1),
                Arguments.of("ab", "bca", 0),
                Arguments.of("ab", "ab", 1),
                Arguments.of("a", "a", 1),
                Arguments.of("a", "b", 0),
                Arguments.of("asdf", "asdfasdf", 1),
                Arguments.of("asdf", "asdfasdfasdf", 1),
                Arguments.of("aa", "aaa", 1),
                Arguments.of("aba", "abaabaabaabaabaabaab", 0)
        );
    }
}

enum InfinityStringChecker {
    INSTANCE;

    public int check(String str1, String str2) {
        return isSame(str1, str2) ? 1 : 0;
    }

    private boolean isSame(String str1, String str2) {
        return str1.repeat(str2.length()).equals(str2.repeat(str1.length()));
    }
}
~~~
