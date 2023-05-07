# 놀라운 문자열 (Silver 3)

### 문제 설명

대문자 알파벳으로만 이루어져 있는 문자열이 있다. 이 문자열에 대해서 ‘D-쌍’이라는 것을 정의할 수 있는데, 이 문자열에 포함되어 있는, 거리가 D인 두 문자를 순서대로 나열한 것을 이 문자열의 D-쌍이라고 한다. 예를 들어 문자열이 ZGBG라고 하자. 이 문자열의 0-쌍은 ZG, GB, BG가 되고, 이 문자열의 1-쌍은 ZB, GG가 되며, 이 문자열의 2-쌍은 ZG가 된다. 문자열의 길이가 N이라고 할 때, 0-쌍부터 (N-2)-쌍까지가 정의됨을 알 수 있다.

만일 정의되는 D에 대해, 어떤 문자열의 모든 D-쌍들이 서로 다를 때, 이 문자열을 D-유일하다고 한다. 위의 예를 보면, 0-쌍들은 ZG, GB, BG로 모두 다르다. 따라서 이 문자열은 0-유일하며, 마찬가지로 1-유일하고, 2-유일하다. 하지만 만일 문자열이 AAA라고 하자. 이 문자열은 0-유일하지 않으며, 다만 1-유일하다.

만일 어떤 문자열이 정의되는 모든 D에 대해 D-유일하면, 이 문자열을 정말이지 ‘놀라운 문자열’이라고 한다. 문자열이 주어질 때, 이 문자열이 놀라운 문자열인지 아닌지를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1972

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        SurprisingStringChecker checker = new SurprisingStringChecker();
        String line;
        while (!(line = br.readLine()).equals("*")) {
            System.out.println(checker.isSurprising(line));
        }
    }
}

class SurprisingStringChecker {

    public String isSurprising(String string) {
        return checkIsSurprising(string) ? string + " is surprising." : string + " is NOT surprising.";
    }

    private boolean checkIsSurprising(String string) {
        return IntStream.range(1, string.length())
                .noneMatch(distance -> containsIsNotSurprising(string, distance));
    }

    private boolean containsIsNotSurprising(String string, int distance) {
        Set<String> dPairs = getDPairsOfDistance(string, distance);
        return dPairs.size() != string.length() - distance;
    }

    private Set<String> getDPairsOfDistance(String string, int distance) {
        Set<String> dPairs = new HashSet<>();
        for (int i = 0; i < string.length() - distance; i++) {
            dPairs.add(String.valueOf(string.charAt(i)) + string.charAt(i + distance));
        }
        return dPairs;
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

import java.util.HashSet;
import java.util.Set;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideStrings")
    @DisplayName("모든 D에 대해 D-유일하면 놀라운 문자열 아니라면 놀랍지 않은 문자열이라고 반환한다.")
    void isSurprising(String string, String expected) {
        SurprisingStringChecker sut = new SurprisingStringChecker();

        String actual = sut.isSurprising(string);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideStrings() {
        return Stream.of(
                Arguments.of("AB", "AB is surprising.")
                , Arguments.of("ABC", "ABC is surprising.")
                , Arguments.of("AAA", "AAA is NOT surprising.")
                , Arguments.of("ZGBG", "ZGBG is surprising.")
                , Arguments.of("X", "X is surprising.")
                , Arguments.of("EE", "EE is surprising.")
                , Arguments.of("AAB", "AAB is surprising.")
                , Arguments.of("AABA", "AABA is surprising.")
                , Arguments.of("AABB", "AABB is NOT surprising.")
                , Arguments.of("BCBABCC", "BCBABCC is NOT surprising.")
        );
    }
}

class SurprisingStringChecker {

    public String isSurprising(String string) {
        return checkIsSurprising(string) ? string + " is surprising." : string + " is NOT surprising.";
    }

    private boolean checkIsSurprising(String string) {
        return IntStream.range(1, string.length())
                .noneMatch(distance -> containsIsNotSurprising(string, distance));
    }

    private boolean containsIsNotSurprising(String string, int distance) {
        Set<String> dPairs = getDPairsOfDistance(string, distance);
        return dPairs.size() != string.length() - distance;
    }

    private Set<String> getDPairsOfDistance(String string, int distance) {
        Set<String> dPairs = new HashSet<>();
        for (int i = 0; i < string.length() - distance; i++) {
            dPairs.add(String.valueOf(string.charAt(i)) + string.charAt(i + distance));
        }
        return dPairs;
    }
}
~~~
