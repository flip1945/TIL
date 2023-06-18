# 창영마을 (Bronze 2)

### 문제 설명

상근이와 정인이는 창영마을에 살고 있다. 창영마을은 전세계에서 가장 평화로운 마을로 알려져 있다. 이 마을의 이장은 상근이다.

정인이는 항상 상근이의 자리를 질투하고 있다. 정인이는 상근이가 이장의 자격이 없다는 것을 속임수를 이용해서 사람들에게 알려주려고 한다. 

먼저 정인이는 불투명한 컵 세 개를 일렬로 탁자 위에 올려놓고, 가장 왼쪽 컵에 작은 공 하나를 넣어놓았다. 이제 정인이는 컵 2개를 위치를 바꿔가면서 여러 번 섞을것이고, 모두 섞은 뒤에 상근이에게 어떤 컵에 공이 들어있는지 말하라고 할 것이다. 컵이 3개가 있을 때, 위치를 바꿀 수 있는 가능한 방법은 아래와 같이 3가지가 있다.

<img src="https://upload.acmicpc.net/dd088d62-0715-46dc-ae95-5d0279156ced/-/preview/" width="530px">

정인이는 이날만을 위해 컵 섞기를 30년간 연습해왔다. 따라서 상근이는 아무리 쳐다보아도 정인이의 팔 속도를 따라갈수 없다.

하지만, 정인이는 상근이가 뛰어난 프로그래머라는 것을 잊고 있었다. 또한, 상근이는 정인이가 언젠간 이런 반란을 일으킬 것을 알았기 때문에, 항상 정인이와 만날 때는 모든 상황을 비디오로 녹화하고 있었다.

정인이가 컵을 섞은 방법이 순서대로 주어질 때, 어떤 컵에 공이 있는지 알아내는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/3028

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ShellGame shellGame = new ShellGame();
        shellGame.play(scanner.nextLine());

        System.out.println(shellGame.getAnswer());
    }
}

class ShellGame {

    public static final int STARTING_BALL_LOCATION = 1;
    private int locationOfBall = STARTING_BALL_LOCATION;

    public void play(String command) {
        for (String cmd : command.split("")) {
            locationOfBall = Command.valueOf(cmd).apply(locationOfBall);
        }
    }

    public int getAnswer() {
        return locationOfBall;
    }
}

enum Command {
    A("change left and center", locationOfBall -> execute(locationOfBall, Constants.RIGHT, Constants.LEFT, Constants.CENTER)),
    B("change center and right", locationOfBall -> execute(locationOfBall, Constants.LEFT, Constants.CENTER, Constants.RIGHT)),
    C("change left and right", locationOfBall -> execute(locationOfBall, Constants.CENTER, Constants.LEFT, Constants.RIGHT));

    private final String description;
    private final IntFunction<Integer> commandFunction;

    Command(String description, IntFunction<Integer> commandFunction) {
        this.description = description;
        this.commandFunction = commandFunction;
    }

    public int apply(int locationOfBall) {
        return commandFunction.apply(locationOfBall);
    }

    private static int execute(int locationOfBall, int excludedLocation, int location1, int location2) {
        if (locationOfBall == excludedLocation) {
            return locationOfBall;
        }
        return swapLocation(locationOfBall, location1, location2);
    }

    private static int swapLocation(int locationOfBall, int location1, int location2) {
        return locationOfBall == location1 ? location2 : location1;
    }

    private static class Constants {
        public static final int LEFT = 1;
        public static final int CENTER = 2;
        public static final int RIGHT = 3;
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

import java.util.function.IntFunction;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideCommands")
    @DisplayName("야바위 게임의 결과를 반환한다.")
    void getCountOfJoiAndIoiTest(String command, int expected) {
        ShellGame sut = new ShellGame();
        sut.play(command);

        int actual = sut.getAnswer();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCommands() {
        return Stream.of(
                Arguments.of("A", 2),
                Arguments.of("AA", 1),
                Arguments.of("C", 3),
                Arguments.of("CC", 1),
                Arguments.of("B", 1),
                Arguments.of("ABB", 2),
                Arguments.of("AB", 3),
                Arguments.of("CBABCACCC", 1)
        );
    }
}

class ShellGame {

    public static final int STARTING_BALL_LOCATION = 1;
    private int locationOfBall = STARTING_BALL_LOCATION;

    public void play(String command) {
        for (String cmd : command.split("")) {
            locationOfBall = Command.valueOf(cmd).apply(locationOfBall);
        }
    }

    public int getAnswer() {
        return locationOfBall;
    }
}

enum Command {
    A("change left and center", locationOfBall -> execute(locationOfBall, Constants.RIGHT, Constants.LEFT, Constants.CENTER)),
    B("change center and right", locationOfBall -> execute(locationOfBall, Constants.LEFT, Constants.CENTER, Constants.RIGHT)),
    C("change left and right", locationOfBall -> execute(locationOfBall, Constants.CENTER, Constants.LEFT, Constants.RIGHT));

    private final String description;
    private final IntFunction<Integer> commandFunction;

    Command(String description, IntFunction<Integer> commandFunction) {
        this.description = description;
        this.commandFunction = commandFunction;
    }

    public int apply(int locationOfBall) {
        return commandFunction.apply(locationOfBall);
    }

    private static int execute(int locationOfBall, int excludedLocation, int location1, int location2) {
        if (locationOfBall == excludedLocation) {
            return locationOfBall;
        }
        return swapLocation(locationOfBall, location1, location2);
    }

    private static int swapLocation(int locationOfBall, int location1, int location2) {
        return locationOfBall == location1 ? location2 : location1;
    }

    private static class Constants {
        public static final int LEFT = 1;
        public static final int CENTER = 2;
        public static final int RIGHT = 3;
    }
}
~~~
