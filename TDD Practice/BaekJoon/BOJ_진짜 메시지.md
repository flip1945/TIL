# 진짜 메시지 (Silver 5)

### 문제 설명

스파이들은 사령부와 통신하기 위해서 SMTP(비밀 메시지 전송 프로토콜)를 사용해 비밀 회선으로 전자 메시지를 보낸다. 메시지가 적들에 의해 조작되어 보내진 것이 아닌 진짜 메시지라는 것을 표시하기 위해 모든 메시지는 회선에 노이즈가 있었거나 발신 측에서 손을 떨면서 메시지를 보낸 것처럼 변형되는데, 이 변형 알고리즘은 메시지를 가로채는 자들이 우연히 변형 규칙을 흉내 낼 수 없을 정도로 정교하다. 또한 요원들의 머리에 총구가 겨눠져 강제로 메시지를 말한 경우 간단히 실수를 의도적으로 넣어 이 메시지가 강제로 쓰인 메시지라는 것을 알려줄 수 있다.

알고리즘대로 정확하게 변형된 메시지는 각 문자가 세 번째 등장할 때 한 번 더 문자가 삽입된다. 예를 들면 요원이 "HELLOTHEREWELLBEFINE" 라는 메시지를 보내고 싶어 했다면 "HELLOTHEREEWELLLBEFINEE" 는 정확한 변형이다. 몇 년 동안 이 메시지들의 진짜 여부는 고도로 훈련된 원숭이들이 판별해내었다. 그러나 사령부에 도착하는 메시지들의 양이 많이 늘어나면서 이 작업을 자동으로 처리해주는 프로그램을 고안하기로 하였다.

출처 : https://www.acmicpc.net/problem/9324

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        RealMessageChecker checker = new RealMessageChecker();
        for (int i = 0; i < n; i++) {
            System.out.println(checker.check(scanner.nextLine()));
        }
    }
}

class RealMessageChecker {
    public static final String REAL_MESSAGE = "OK";
    public static final String FAKE_MESSAGE = "FAKE";
    public static final char PADDING = ' ';
    public static final int DEFAULT_VALUE = -1;

    public String check(String message) {
        Map<Character, Integer> counter = new HashMap<>();
        char prev = PADDING;
        for (char letter : getPaddingMessage(message).toCharArray()) {
            if (counter.getOrDefault(prev, DEFAULT_VALUE) == 3) {
                if (letter != prev) {
                    return FAKE_MESSAGE;
                }
                counter.put(letter, 0);
                continue;
            }
            counter.put(letter, counter.getOrDefault(letter, 0) + 1);
            prev = letter;
        }
        return REAL_MESSAGE;
    }

    private String getPaddingMessage(String message) {
        return message + PADDING;
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
    @MethodSource("provideMessages")
    @DisplayName("진짜 메시지 인지 확인한다.")
    void it_checks_real_message(String message, String expected) {
        RealMessageChecker sut = new RealMessageChecker();

        String actual = sut.check(message);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideMessages() {
        return Stream.of(
                Arguments.of("AB", "OK"),
                Arguments.of("AAA", "FAKE"),
                Arguments.of("AAAAAAAA", "OK"),
                Arguments.of("BAPC", "OK"),
                Arguments.of("AABA", "FAKE"),
                Arguments.of("ABCABCBBAAACC", "OK"),
                Arguments.of("ABABABAB", "FAKE"),
                Arguments.of("AAAAAA", "OK")
        );
    }
}

class RealMessageChecker {
    public static final String REAL_MESSAGE = "OK";
    public static final String FAKE_MESSAGE = "FAKE";
    public static final char PADDING = ' ';
    public static final int DEFAULT_VALUE = -1;

    public String check(String message) {
        Map<Character, Integer> counter = new HashMap<>();
        char prev = PADDING;
        for (char letter : getPaddingMessage(message).toCharArray()) {
            if (counter.getOrDefault(prev, DEFAULT_VALUE) == 3) {
                if (letter != prev) {
                    return FAKE_MESSAGE;
                }
                counter.put(letter, 0);
                continue;
            }
            counter.put(letter, counter.getOrDefault(letter, 0) + 1);
            prev = letter;
        }
        return REAL_MESSAGE;
    }

    private String getPaddingMessage(String message) {
        return message + PADDING;
    }
}
~~~
