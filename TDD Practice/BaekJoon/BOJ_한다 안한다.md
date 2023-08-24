# 한다 안한다 (Bronze 3)

### 문제 설명

옛날에는 결정하기 어려운 일이 있을 때는 꽃을 이용해서 결정을 내렸다. 꽃을 하나 떼서 잎을 하나씩 떼면서, 한다와 안한다를 번갈아 가면서 말하다가 마지막 잎을 뗄 때 말한 말로 결정을 했다.

상근이는 이 방법을 응용해서 결정하기 어려운 일을 하나 결정하려고 한다.

먼저, 0과 1로 이루어진 문자열을 랜덤으로 하나 만든다. 그 다음 문자열의 양 끝에서 수를 하나씩 고르고, 두 수를 비교한다. 수가 같으면 "한다"이고, 다르면 "안한다"이다. 그 다음에는 고른 수를 버리고, 모든 수를 고를 때까지 이 작업을 반복한다. 따라서, 마지막으로 고르는 두 숫자로 결정을 내리는 것이다.

0과 1로 이루어진 문자열이 주어졌을 때, 상근이가 내리는 결정을 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/5789

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            PetalDivination petalDivination = new PetalDivination(scanner.nextLine());
            System.out.println(petalDivination.makeDecision());
        }
    }
}

class PetalDivination {

    private static final String TRUE_STRING = "Do-it";
    private static final String FALSE_STRING = "Do-it-Not";

    private final String petals;

    public PetalDivination(String petals) {
        this.petals = petals;
    }

    public String makeDecision() {
        return isSameShape() ? TRUE_STRING : FALSE_STRING;
    }

    private boolean isSameShape() {
        return petals.charAt(getFirstPetalIndex()) == petals.charAt(getSecondPetalIndex());
    }

    private int getFirstPetalIndex() {
        return petals.length() / 2;
    }

    private int getSecondPetalIndex() {
        return getFirstPetalIndex() - 1;
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
    @MethodSource("providePetals")
    @DisplayName("꽃잎 점으로 결과를 정한다.")
    void shouldMakeDecision(String petals, String expected) {
        PetalDivination sut = new PetalDivination(petals);

        String actual = sut.makeDecision();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> providePetals() {
        return Stream.of(
                Arguments.of("11", "Do-it"),
                Arguments.of("10", "Do-it-Not"),
                Arguments.of("00", "Do-it"),
                Arguments.of("1001", "Do-it"),
                Arguments.of("00100010", "Do-it"),
                Arguments.of("01010101", "Do-it-Not"),
                Arguments.of("100001", "Do-it")
        );
    }
}

class PetalDivination {

    private static final String TRUE_STRING = "Do-it";
    private static final String FALSE_STRING = "Do-it-Not";

    private final String petals;

    public PetalDivination(String petals) {
        this.petals = petals;
    }

    public String makeDecision() {
        return isSameShape() ? TRUE_STRING : FALSE_STRING;
    }

    private boolean isSameShape() {
        return petals.charAt(getFirstPetalIndex()) == petals.charAt(getSecondPetalIndex());
    }

    private int getFirstPetalIndex() {
        return petals.length() / 2;
    }

    private int getSecondPetalIndex() {
        return getFirstPetalIndex() - 1;
    }
}
~~~
