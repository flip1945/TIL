# 야바위 대장 (Bronze 2)

### 문제 설명

10년 동안 도박판에서 야바위를 한 영훈은 이제 보지 않고도 구슬이 있는 컵을 맞추는 지경에 이르렀다.

이런 영훈을 골탕 먹이기 위해 문자열로 야바위를 하려고 한다.

T번 동안 문자열 S의 A번째 위치에 있는 문자와 B번째 위치에 있는 문자를 바꾼 결과를 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/15814

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String word = scanner.nextLine();
        int n = Integer.parseInt(scanner.nextLine());
        List<String> commands = inputCommands(scanner, n);

        ShellGame shellGame = new ShellGame(word);
        shellGame.play(commands);
        System.out.println(shellGame.getAnswer());
    }

    private static List<String> inputCommands(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class ShellGame {

    public static final String DELIMITER = "";
    private final String[] letters;

    public ShellGame(String word) {
        this.letters = word.split(DELIMITER);
    }

    public void play(List<String> commands) {
        commands.forEach(this::executeCommand);
    }

    public String getAnswer() {
        return String.join(DELIMITER, letters);
    }

    private void executeCommand(String command) {
        swap(SwapCommand.from(command));
    }

    private void swap(SwapCommand command) {
        String temp = letters[command.getLeft()];
        letters[command.getLeft()] = letters[command.getRight()];
        letters[command.getRight()] = temp;
    }
}

class SwapCommand {

    public static final String DELIMITER = " ";
    public static final int LEFT_COMMAND_INDEX = 0;
    public static final int RIGHT_COMMAND_INDEX = 1;
    private final int left;
    private final int right;

    private SwapCommand(String command) {
        String[] cmd = command.split(DELIMITER);
        this.left = Integer.parseInt(cmd[LEFT_COMMAND_INDEX]);
        this.right = Integer.parseInt(cmd[RIGHT_COMMAND_INDEX]);
    }

    public static SwapCommand from(String command) {
        return new SwapCommand(command);
    }

    public int getLeft() {
        return left;
    }

    public int getRight() {
        return right;
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
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideWords")
    @DisplayName("문자 위치를 변경하는 명령 목록을 받아 바뀐 문자열의 결과를 반환한다.")
    void shellGameTest(String word, List<String> commands, String expected) {
        ShellGame sut = new ShellGame(word);
        sut.play(commands);

        String actual = sut.getAnswer();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideWords() {
        return Stream.of(
                Arguments.of("abc", List.of("1 2"), "acb"),
                Arguments.of("abc", List.of("0 1"), "bac"),
                Arguments.of("abc", List.of("0 1", "1 2"), "bca"),
                Arguments.of("a", List.of("0 0"), "a"),
                Arguments.of("Youngmaan-good", List.of("1 3", "9 2"), "Yn-ogmaanugood")
        );
    }
}

class ShellGame {

    public static final String DELIMITER = "";
    private final String[] letters;

    public ShellGame(String word) {
        this.letters = word.split(DELIMITER);
    }

    public void play(List<String> commands) {
        commands.forEach(this::executeCommand);
    }

    public String getAnswer() {
        return String.join(DELIMITER, letters);
    }

    private void executeCommand(String command) {
        swap(SwapCommand.from(command));
    }

    private void swap(SwapCommand command) {
        String temp = letters[command.getLeft()];
        letters[command.getLeft()] = letters[command.getRight()];
        letters[command.getRight()] = temp;
    }
}

class SwapCommand {

    public static final String DELIMITER = " ";
    public static final int LEFT_COMMAND_INDEX = 0;
    public static final int RIGHT_COMMAND_INDEX = 1;
    private final int left;
    private final int right;

    private SwapCommand(String command) {
        String[] cmd = command.split(DELIMITER);
        this.left = Integer.parseInt(cmd[LEFT_COMMAND_INDEX]);
        this.right = Integer.parseInt(cmd[RIGHT_COMMAND_INDEX]);
    }

    public static SwapCommand from(String command) {
        return new SwapCommand(command);
    }

    public int getLeft() {
        return left;
    }

    public int getRight() {
        return right;
    }
}
~~~
