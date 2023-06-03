# 닉네임에 갓 붙이기 (Bronze 2)

### 문제 설명

방금 막 프로그래밍을 배우기 시작한 찬우는 acmicpc.net에 있는 회원들이 모두 신같이 보였다. 그래서 찬우는 모든 회원의 닉네임 앞에 ‘갓’을 붙이려고 한다.

찬우가 ‘koosaga’라는 닉네임을 가진 회원을 갓으로 바꿔 부른다면 ‘godsaga’가 된다. 또, 찬우가 ‘acka’라는 닉네임을 가진 회원을 갓으로 바꿔 부른다면 ‘godka’가 될 것이다. 찬우는 닉네임을 갓으로 바꾸는 알고리즘을 생각하다가, 아래와 같이 2단계 방법을 사용하면 될 것으로 생각했다.

* 닉네임을 음절 단위로 쪼갠다.
* 가장 첫 음절을 ‘god’으로 바꾼 후 쪼갠 음절을 합친다.

찬우는 수작업으로 N명의 닉네임을 모두 음절 단위로 쪼갰다. 찬우를 도와 이 닉네임들에 갓을 붙이는 프로그램을 작성하자.

출처 : https://www.acmicpc.net/problem/13163

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            System.out.println(GodMaker.make(scanner.nextLine()));
        }
    }
}

class GodMaker {
    private static final String GOD_PREFIX = "god";
    private static final String JOIN_DELIMITER = "";
    private static final String SYLLABLE_DELIMITER = " ";
    private static final int SYLLABLE_FROM_INDEX = 1;

    public static String make(String nickname) {
        return makeGodNickname(getSyllables(nickname));
    }

    private static String makeGodNickname(List<String> syllables) {
        return GOD_PREFIX + String.join(JOIN_DELIMITER, syllables.subList(SYLLABLE_FROM_INDEX, syllables.size()));
    }

    private static List<String> getSyllables(String nickname) {
        return Arrays.asList(nickname.split(SYLLABLE_DELIMITER));
    }
}
~~~

---

#### 테스트 코드
~~~java
package test;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

import static org.junit.jupiter.api.Assertions.*;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

public class MainTest {

    @ParameterizedTest
    @MethodSource("provideNicknames")
    @DisplayName("갓을 붙인 닉네임을 반환한다.")
    void it_returns_nickname_of_god_prefix(String nickname, String expected) {
        String actual = GodMaker.make(nickname);
        
        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNicknames() {
        return Stream.of(
            Arguments.of("a b", "godb"),
            Arguments.of("beak joon", "godjoon"),
            Arguments.of("koo sa ga", "godsaga"),
            Arguments.of("ac ka", "godka"),
            Arguments.of("yu ka ri ko", "godkariko"),
            Arguments.of("ke sa ki yo", "godsakiyo"),
            Arguments.of("a a", "goda")
        );
    }
}

class GodMaker {
    private static final String GOD_PREFIX = "god";
    private static final String JOIN_DELIMITER = "";
    private static final String SYLLABLE_DELIMITER = " ";
    private static final int SYLLABLE_FROM_INDEX = 1;

    public static String make(String nickname) {
        return makeGodNickname(getSyllables(nickname));
    }

    private static String makeGodNickname(List<String> syllables) {
        return GOD_PREFIX + String.join(JOIN_DELIMITER, syllables.subList(SYLLABLE_FROM_INDEX, syllables.size()));
    }

    private static List<String> getSyllables(String nickname) {
        return Arrays.asList(nickname.split(SYLLABLE_DELIMITER));
    }
}
~~~
