# Greetings! (Bronze 3)

### 문제 설명

Now that Snapchat and Slingshot are soooo 2018, the teenagers of the world have all switched to the new hot app called BAPC (Bidirectional and Private Communication). This app has some stricter social rules than previous iterations. For example, if someone says goodbye using Later!, the other person is expected to reply with Alligator!. You can not keep track of all these social conventions and decide to automate any necessary responses, starting with the most important one: the greetings. When your conversational partner opens with he...ey, you have to respond with hee...eey as well, but using twice as many e’s!

Given a string of the form he...ey of length at most 1000, print the greeting you will respond with, containing twice as many e’s.

출처 : https://www.acmicpc.net/problem/17548

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        GoodBye goodBye = new GoodBye(scanner.nextLine());
        System.out.println(goodBye.bye());
    }
}

class GoodBye {

    private static final String BYE_PREFIX = "h";
    private static final String BYE_SUFFIX = "y";
    private static final String E = "e";

    private final String hey;

    public GoodBye(String hey) {
        this.hey = hey;
    }

    public String bye() {
        return BYE_PREFIX + getE() + BYE_SUFFIX;
    }

    private String getE() {
        return E.repeat(getESize() * 2);
    }

    private int getESize() {
        return hey.length() - 2;
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
    @MethodSource("provideHey")
    @DisplayName("인삿말이 주어지면 e가 두 배로 많은 인삿말로 반환한다.")
    void goodByeReturnsByeWithTwiceE(String hey, String expected) {
        GoodBye sut = new GoodBye(hey);

        String actual = sut.bye();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideHey() {
        return Stream.of(
                Arguments.of("hey", "heey"),
                Arguments.of("heey", "heeeey"),
                Arguments.of("heeeeey", "heeeeeeeeeey")
        );
    }
}

class GoodBye {

    private static final String BYE_PREFIX = "h";
    private static final String BYE_SUFFIX = "y";
    private static final String E = "e";

    private final String hey;

    public GoodBye(String hey) {
        this.hey = hey;
    }

    public String bye() {
        return BYE_PREFIX + getE() + BYE_SUFFIX;
    }

    private String getE() {
        return E.repeat(getESize() * 2);
    }

    private int getESize() {
        return hey.length() - 2;
    }
}
~~~
