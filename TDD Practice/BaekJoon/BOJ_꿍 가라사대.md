# 꿍 가라사대 (Bronze 2)

### 문제 설명

영어공부를 열심히 하고 있는 꿍이 대학교MT에 놀러가서 친해지고 싶은 후배들과 Simon Says 게임을 하려고 한다.

"Simon Says" 게임의 룰은 간단하다. 만약 어떤 사람이 "Simon says"라고 말한 후 어떤 지시사항을 이야기했다면 모두 그 지시사항을 따라 하기만 하면 된다. 예를 들어 영어를 잘하는 꿍이 "Simon says touch your nose"라고 말한다면 모두 코를 만져야한다. 하지만 이때, 꿍이 "Stop touching your nose" 라고 말했을 때 누군가 코를 만지는 것을 멈춘다면 그 플레이어는 벌칙을 받아야 한다.

즉, Simon says 라고 말한 후에 나온 지시사항만을 따라야 하는 것이다.

하지만 똑똑한 컴공 후배들은 절대 실수할리 없는 컴퓨터 프로그램을 짜서 이 게임을 하려고 한다.

출처 : https://www.acmicpc.net/problem/11094

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        SimonSays simonSays = new SimonSays(inputCommands(scanner, n));
        printSimonSays(simonSays);
    }

    private static void printSimonSays(SimonSays simonSays) {
        simonSays.getSimonSays()
                .forEach(System.out::println);
    }

    private static List<String> inputCommands(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class SimonSays {

    private final List<String> commands;

    public SimonSays(List<String> commands) {
        this.commands = commands;
    }

    public List<String> getSimonSays() {
        return commands.stream()
                .map(SimonSay::new)
                .filter(SimonSay::isSimonSay)
                .map(SimonSay::getSimonSayCommand)
                .collect(Collectors.toList());
    }
}

class SimonSay {

    public static final String SIMON_SAYS = "Simon says";
    public static final int COMMAND_BEGIN_INDEX = 10;

    private final String command;

    public SimonSay(String command) {
        this.command = command;
    }

    public boolean isSimonSay() {
        return command.startsWith(SIMON_SAYS);
    }

    public String getSimonSayCommand() {
        return command.substring(COMMAND_BEGIN_INDEX);
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
    @MethodSource("provideCommands")
    @DisplayName("Simon says 이후에 나오는 문장만 모아서 반환한다.")
    void testGetSimonSays(List<String> commands, List<String> expected) {
        SimonSays sut = new SimonSays(commands);

        List<String> actual = sut.getSimonSays();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCommands() {
        return Stream.of(
                Arguments.of(List.of("a."), List.of()),
                Arguments.of(List.of("Simon says a."), List.of(" a.")),
                Arguments.of(List.of("Simon says b."), List.of(" b.")),
                Arguments.of(List.of("Simon says a.", "Simon says b."), List.of(" a.", " b.")),
                Arguments.of(List.of("Simon says b.", "Simon says a.", "a."), List.of(" b.", " a.")),
                Arguments.of(List.of("Simon says smile."), List.of(" smile.")),
                Arguments.of(List.of("Simon says raise your right hand.", "Lower your right hand.", "Simon says raise your left hand."), List.of(" raise your right hand.", " raise your left hand.")),
                Arguments.of(List.of("Raise your right hand.", "Lower your right hand.", "Simon says raise your left hand."), List.of(" raise your left hand."))
        );
    }
}

class SimonSays {

    private final List<String> commands;

    public SimonSays(List<String> commands) {
        this.commands = commands;
    }

    public List<String> getSimonSays() {
        return commands.stream()
                .map(SimonSay::new)
                .filter(SimonSay::isSimonSay)
                .map(SimonSay::getSimonSayCommand)
                .collect(Collectors.toList());
    }
}

class SimonSay {

    public static final String SIMON_SAYS = "Simon says";
    public static final int COMMAND_BEGIN_INDEX = 10;

    private final String command;

    public SimonSay(String command) {
        this.command = command;
    }

    public boolean isSimonSay() {
        return command.startsWith(SIMON_SAYS);
    }

    public String getSimonSayCommand() {
        return command.substring(COMMAND_BEGIN_INDEX);
    }
}
~~~
