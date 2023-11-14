# 이제는 더 이상 물러날 곳이 없다 (Bronze 1)

### 문제 설명

건덕이와 건구스는 $N$개의 칸이 가로로 놓인 전장에서 승부를 겨루고 있다. 처음에는 가장 왼쪽 칸에 건덕이가, 가장 오른쪽 칸에 건구스가 자리 잡고 있으며, 승자는 아래 규칙에 따라 정해진다.

1. 자신의 차례에 아래 두 가지 행동 중 하나를 반드시 수행해야 한다. 전장을 벗어나도록 이동할 수 없으며, 행동을 마친 뒤에는 상대방의 차례가 된다.
    * 좌우로 인접한 칸으로 이동한다.
    * 좌우로 인접한 칸에 상대방이 있다면, 상대방을 공격한다.
2. 상대방을 공격하는 경우 승리한다.

전장의 크기가 주어졌을 때, 누가 승리하는지 판단하자. 둘 다 최선을 다해서 승부를 겨루며, 처음 행동을 수행하는 사람은 건덕이다.

출처 : https://www.acmicpc.net/problem/30455

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int fieldSize = scanner.nextInt();
        GooseGooseDuck gooseGooseDuck = new GooseGooseDuck(fieldSize);
        System.out.println(gooseGooseDuck.winner());
    }
}

class GooseGooseDuck {

    private static final String GUN_DUCK = "Duck";
    private static final String GUN_GOOSE = "Goose";

    private final int fieldSize;

    public GooseGooseDuck(int fieldSize) {
        this.fieldSize = fieldSize;
    }

    public String winner() {
        return doseGunDuckWin() ? GUN_DUCK : GUN_GOOSE;
    }

    private boolean doseGunDuckWin() {
        return fieldSize % 2 == 0;
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
    @MethodSource("provideField")
    @DisplayName("전장의 크기가 주어지면 이기는 사람을 반환한다.")
    void gooseGooseDuckReturnsWinnerGivenFieldSize(int fieldSize, String expected) {
        GooseGooseDuck sut = new GooseGooseDuck(fieldSize);

        String actual = sut.winner();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideField() {
        return Stream.of(
                Arguments.of(3, "Goose"),
                Arguments.of(4, "Duck"),
                Arguments.of(7, "Goose")
        );
    }
}

class GooseGooseDuck {

    private static final String GUN_DUCK = "Duck";
    private static final String GUN_GOOSE = "Goose";

    private final int fieldSize;

    public GooseGooseDuck(int fieldSize) {
        this.fieldSize = fieldSize;
    }

    public String winner() {
        return doseGunDuckWin() ? GUN_DUCK : GUN_GOOSE;
    }

    private boolean doseGunDuckWin() {
        return fieldSize % 2 == 0;
    }
}
~~~
