# 앵그리 창영 (Bronze 3)

### 문제 설명

창영이는 화가나서 성냥을 바닥에 던졌다.

상근이는 바닥이 더러워진 것을 보고 창영이를 매우 혼냈다.

강산이는 근처에서 박스를 발견했다.

상덕이는 강산이가 발견한 박스를 상근이에게 주었다.

상근이는 박스에 던진 성냥을 모두 담아오라고 시켰다.

하지만, 박스에 들어가지 않는 성냥도 있다.

이런 성냥은 박스에 담지 않고 희원이에게 줄 것이다.

성냥이 박스에 들어가려면, 박스의 밑면에 성냥이 모두 닿아야 한다.

박스의 크기와 성냥의 길이가 주어졌을 때, 성냥이 박스에 들어갈 수 있는지 없는지를 구하는 프로그램을 작성하시오. 창영이는 성냥을 하나씩 검사한다.

출처 : https://www.acmicpc.net/problem/3034

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        MatchBox matchBox = new MatchBox(scanner.nextInt(), scanner.nextInt());

        for (int i = 0; i < n; i++) {
            System.out.println(matchBox.canFitIn(scanner.nextInt()));
        }
    }
}

class MatchBox {

    private static final String FIT = "DA";
    private static final String NOT_FIT = "NE";

    private final int width;
    private final int height;

    public MatchBox(int width, int height) {
        this.width = width;
        this.height = height;
    }

    public String canFitIn(int lengthOfMatch) {
        return lengthOfMatch <= getLengthOfDiagonal() ? FIT : NOT_FIT;
    }

    private double getLengthOfDiagonal() {
        return Math.sqrt((double)width * width + height * height);
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
    @MethodSource("provideMatches")
    @DisplayName("성냥 박스의 가로와 세로 크기, 성냥의 길이가 주어지면 성냥이 박스에 들어갈 수 있는 지 확인한다.")
    void matchBoxChecksMatchFitInTheBoxGivenWidthAndHeight(int width, int height, int lengthOfMatch, String expected) {
        MatchBox sut = new MatchBox(width, height);

        String actual = sut.canFitIn(lengthOfMatch);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideMatches() {
        return Stream.of(
                Arguments.of(1, 1, 1, "DA"),
                Arguments.of(1, 1, 2, "NE"),
                Arguments.of(3, 3, 4, "DA"),
                Arguments.of(3, 4, 3, "DA"),
                Arguments.of(3, 4, 4, "DA"),
                Arguments.of(3, 4, 5, "DA"),
                Arguments.of(3, 4, 6, "NE"),
                Arguments.of(3, 4, 7, "NE"),
                Arguments.of(12, 17, 20, "DA"),
                Arguments.of(12, 17, 21, "NE")
        );
    }
}

class MatchBox {

    private static final String FIT = "DA";
    private static final String NOT_FIT = "NE";

    private final int width;
    private final int height;

    public MatchBox(int width, int height) {
        this.width = width;
        this.height = height;
    }

    public String canFitIn(int lengthOfMatch) {
        return lengthOfMatch <= getLengthOfDiagonal() ? FIT : NOT_FIT;
    }

    private double getLengthOfDiagonal() {
        return Math.sqrt((double)width * width + height * height);
    }
}
~~~
