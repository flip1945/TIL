# Football Team (Bronze 4)

### 문제 설명

The PLU football coach must submit to the NCAA officials the names of all players that will be competing in NCAA Division II championship game. Unfortunately his computer keyboard malfunctioned and interchanged the letters ‘i’ and ‘e’. Your job is to write a program that will read all the names and print the names with the correct spelling.

출처 : https://www.acmicpc.net/problem/5358

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNextLine()) {
            String name = scanner.nextLine();
            Player player = new Player(name);
            System.out.println(player.getReplacedName());
        }
    }
}

class Player {

    private final String name;

    public Player(String name) {
        this.name = name;
    }

    public String getReplacedName() {
        return name.chars()
                .mapToObj(letter -> replaceLetter((char) letter))
                .collect(Collectors.joining());
    }

    private String replaceLetter(char letter) {
        switch (letter) {
            case 'i':
                return "e";
            case 'I':
                return "E";
            case 'e':
                return "i";
            case 'E':
                return "I";
            default:
                return String.valueOf(letter);
        }
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

import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePlayer")
    @DisplayName("선수의 이름이 주어지면 i와 e를 바꾼 이름을 반환한다.")
    void playerReturnsReplacedName(String name, String expected) {
        Player sut = new Player(name);

        String actual = sut.getReplacedName();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> providePlayer() {
        return Stream.of(
                Arguments.of("Aim", "Aem"),
                Arguments.of("Bit", "Bet"),
                Arguments.of("BIT", "BET"),
                Arguments.of("Tem Tibow", "Tim Tebow"),
                Arguments.of("Ember White", "Imbir Wheti")
        );
    }
}

class Player {

    private final String name;

    public Player(String name) {
        this.name = name;
    }

    public String getReplacedName() {
        return name.chars()
                .mapToObj(letter -> replaceLetter((char) letter))
                .collect(Collectors.joining());
    }

    private String replaceLetter(char letter) {
        switch (letter) {
            case 'i':
                return "e";
            case 'I':
                return "E";
            case 'e':
                return "i";
            case 'E':
                return "I";
            default:
                return String.valueOf(letter);
        }
    }
}
~~~
