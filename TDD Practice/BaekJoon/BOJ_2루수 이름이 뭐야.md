# 2루수 이름이 뭐야 (Bronze 3)

### 문제 설명

1루수가 누구인지 밝혀내는 과정에서, 2루수가 거짓말을 하고 있다는 사실이 드러났다!

이에 극대노한 선수들은 2루수를 찾아내어 혼내주려고 한다. 그런데 이번에는 2루수의 이름을 모른다! 하지만 감독님은 무엇인가 알고 계신 듯하다.

"1루수가 누구야 2루수 이름이 뭐야 3루수는 몰라"

야구팀의 선수 리스트를 보고, 뭐가 있는지 찾아보자.

**2루수 이름이 뭐야**

출처 : https://www.acmicpc.net/problem/17350

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> playerNames = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());

        SecondBasemanChecker checker = new SecondBasemanChecker();
        System.out.println(checker.check(playerNames));
    }
}

class SecondBasemanChecker {

    private static final String SECOND_BASEMAN_NAME = "anj";
    private static final String iS_SECOND_BASEMAN = "뭐야;";
    private static final String IS_NOT_SECOND_BASEMAN = "뭐야?";

    public String check(List<String> playerNames) {
        for (String playerName : playerNames) {
            if (playerName.equals(SECOND_BASEMAN_NAME)) {
                return iS_SECOND_BASEMAN;
            }
        }
        return IS_NOT_SECOND_BASEMAN;
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

import java.util.*;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideUnits")
    @DisplayName("2루수인지 확인해서 있다면 뭐야;를 없다면 뭐야?를 반환해야 한다.")
    void shouldCheckSecondBaseman(List<String> playerNames, String expected) {
        SecondBasemanChecker sut = new SecondBasemanChecker();

        String actual = sut.check(playerNames);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideUnits() {
        return Stream.of(
                Arguments.of(List.of("a"), "뭐야?"),
                Arguments.of(List.of("anj"), "뭐야;"),
                Arguments.of(List.of("a", "anj"), "뭐야;"),
                Arguments.of(List.of("snrn", "anj", "ahffk"), "뭐야;"),
                Arguments.of(List.of("aptl", "molamolamolamola", "dlqmfkglahqlcl", "QWERTOP"), "뭐야?")
        );
    }
}

class SecondBasemanChecker {

    private static final String SECOND_BASEMAN_NAME = "anj";
    private static final String iS_SECOND_BASEMAN = "뭐야;";
    private static final String IS_NOT_SECOND_BASEMAN = "뭐야?";

    public String check(List<String> playerNames) {
        for (String playerName : playerNames) {
            if (playerName.equals(SECOND_BASEMAN_NAME)) {
                return iS_SECOND_BASEMAN;
            }
        }
        return IS_NOT_SECOND_BASEMAN;
    }
}
~~~
