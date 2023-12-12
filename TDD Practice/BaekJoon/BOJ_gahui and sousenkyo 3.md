# gahui and sousenkyo 3 (Bronze 4)

### 문제 설명

Gahui is watching the annual character election. After the election, The top 16 characters receive enormous benefits for one year. For that reason, fans vote passionately to get their favorite characters into the top 16. Remarkably, at least one Cinderella appears in every election, achieving an outstanding outcome.

Election results are announced twice: preliminary and final results. The vote count information for character x is as follows:

* Character x obtained $p_x$ votes when announcing the preliminary result.
* Character x obtained $r_x$ votes when announcing the final result.
 
$v_x$ is defined as $p_x$ divided by $r_x$. The type of character x is determined by the value $v_x$.

* If $v_x \lt 0.2$, the type of character x is weak.
* If $0.2 \le v_x \lt 0.4$, the type of character x is normal.
* If $0.4 \le v_x \lt 0.6$, the type of character x is strong.
* If $0.6 \le v_x$, the type of character x is very strong.

Preliminary and final results of character x are given. Print the type of character x.

출처 : https://www.acmicpc.net/problem/30793

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        CharacterTournament tournament = new CharacterTournament(scanner.nextInt(), scanner.nextInt());
        System.out.println(tournament.getCharacterType());
    }
}

class CharacterTournament {

    private final int preliminaryResult;
    private final int finalResult;

    public CharacterTournament(int preliminaryResult, int finalResult) {
        this.preliminaryResult = preliminaryResult;
        this.finalResult = finalResult;
    }

    public String getCharacterType() {
        return CharacterType.from(getValue()).getType();
    }

    private double getValue() {
        return (double) preliminaryResult / finalResult;
    }
}

enum CharacterType {
    WEAK("weak", value -> value < 0.2),
    NORMAL("normal", value -> value >= 0.2 && value < 0.4),
    STRONG("strong", value -> value >= 0.4 && value < 0.6),
    VERY_STRONG("very strong", value -> value >= 0.6);

    private final String type;
    private final Predicate<Double> matcher;

    CharacterType(String type, Predicate<Double> matcher) {
        this.type = type;
        this.matcher = matcher;
    }

    public static CharacterType from(double characterValue) {
        return Arrays.stream(values())
                .filter(value -> value.matcher.test(characterValue))
                .findFirst()
                .orElse(WEAK);
    }

    public String getType() {
        return type;
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

import java.util.Arrays;
import java.util.function.Predicate;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePreliminaryResultAndFinalResult")
    @DisplayName("예비 결과와 최종 결과가 주어지면 캐릭터의 타입을 반환한다.")
    void characterTournamentReturnsCharacterTypeGivenPreliminaryResultAndFinalResult(int preliminaryResult, int finalResult, String expected) {
        CharacterTournament sut = new CharacterTournament(preliminaryResult, finalResult);

        String actual = sut.getCharacterType();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> providePreliminaryResultAndFinalResult() {
        return Stream.of(
                Arguments.of(19, 100, "weak"),
                Arguments.of(20, 100, "normal"),
                Arguments.of(39, 100, "normal"),
                Arguments.of(40, 100, "strong"),
                Arguments.of(59, 100, "strong"),
                Arguments.of(60, 100, "very strong"),
                Arguments.of(16, 80, "normal"),
                Arguments.of(1884, 4710, "strong")
        );
    }
}

class CharacterTournament {

    private final int preliminaryResult;
    private final int finalResult;

    public CharacterTournament(int preliminaryResult, int finalResult) {
        this.preliminaryResult = preliminaryResult;
        this.finalResult = finalResult;
    }

    public String getCharacterType() {
        return CharacterType.from(getValue()).getType();
    }

    private double getValue() {
        return (double) preliminaryResult / finalResult;
    }
}

enum CharacterType {
    WEAK("weak", value -> value < 0.2),
    NORMAL("normal", value -> value >= 0.2 && value < 0.4),
    STRONG("strong", value -> value >= 0.4 && value < 0.6),
    VERY_STRONG("very strong", value -> value >= 0.6);

    private final String type;
    private final Predicate<Double> matcher;

    CharacterType(String type, Predicate<Double> matcher) {
        this.type = type;
        this.matcher = matcher;
    }

    public static CharacterType from(double characterValue) {
        return Arrays.stream(values())
                .filter(value -> value.matcher.test(characterValue))
                .findFirst()
                .orElse(WEAK);
    }

    public String getType() {
        return type;
    }
}
~~~
