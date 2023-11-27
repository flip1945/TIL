# Previous Level (Bronze 4)

### 문제 설명

메이플스토리라는 RPG 게임에는 레벨이 존재한다.

각각 만렙 = $300$, 구만렙 = $275$, 뀨만렙 = $250$이라는 용어가 존재한다.

모든 레벨은 만렙(구간 1), 구만렙이상 만렙 미만(구간 2), 뀨만렙이상 구만렙 미만(구간 3), 뀨만렙 미만(구간 4) 에 속하게 된다.

 
$N$개의 레벨 $M$이 주어질 때, 각 레벨이 속한 구간의 번호를 출력해 보자.

출처 : https://www.acmicpc.net/problem/28453

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.Predicate;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        Levels levels = Levels.from(inputLevels(scanner, n));

        System.out.println(levels.getZones());
    }

    private static List<Integer> inputLevels(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class Levels {

    private static final String DELIMITER = " ";

    private final List<Level> levels;

    public Levels(List<Level> levels) {
        this.levels = levels;
    }

    public static Levels from(List<Integer> levels) {
        return new Levels(initLevels(levels));
    }

    private static List<Level> initLevels(List<Integer> levels) {
        return levels.stream()
                .map(Level::new)
                .collect(Collectors.toList());
    }

    public String getZones() {
        return levels.stream()
                .map(Level::getZone)
                .map(String::valueOf)
                .collect(Collectors.joining(DELIMITER));
    }
}

class Level {

    private final int level;

    public Level(int level) {
        this.level = level;
    }

    public int getZone() {
        return Zone.from(level).getZone();
    }
}

enum Zone {

    MAX_LEVEL_ZONE(1, level -> level == 300),
    PREVIOUS_MAX_LEVEL_ZONE(2, level -> level >= 275 && level < 300),
    FORMER_MAX_LEVEL_ZONE(3, level -> level >= 250 && level < 275),
    OTHER_LEVEL_ZONE(4, level -> true);

    private final int zone;
    private final Predicate<Integer> matcher;

    Zone(int zone, Predicate<Integer> matcher) {
        this.zone = zone;
        this.matcher = matcher;
    }

    public static Zone from(int level) {
        return Arrays.stream(values())
                .filter(zone -> zone.matcher.test(level))
                .findFirst()
                .orElse(OTHER_LEVEL_ZONE);
    }

    public int getZone() {
        return zone;
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
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideLevel")
    @DisplayName("레벨이 주어지면 레벨이 속한 구간을 반환한다.")
    void levelReturnsZone(int level, int expected) {
        Level sut = new Level(level);

        int actual = sut.getZone();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideLevel() {
        return Stream.of(
                Arguments.of(300, 1),
                Arguments.of(299, 2),
                Arguments.of(275, 2),
                Arguments.of(274, 3),
                Arguments.of(250, 3),
                Arguments.of(249, 4),
                Arguments.of(1, 4)
        );
    }

    @ParameterizedTest
    @MethodSource("provideLevels")
    @DisplayName("레벨들이 주어지면 레벨이 속한 구간들을 반환한다.")
    void levelsReturnsZones(List<Integer> levels, String expected) {
        Levels sut = Levels.from(levels);

        String actual = sut.getZones();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideLevels() {
        return Stream.of(
                Arguments.of(List.of(220, 260, 263, 275, 300), "4 3 3 2 1"),
                Arguments.of(List.of(300, 275, 250, 200), "1 2 3 4")
        );
    }
}

class Levels {

    private static final String DELIMITER = " ";

    private final List<Level> levels;

    public Levels(List<Level> levels) {
        this.levels = levels;
    }

    public static Levels from(List<Integer> levels) {
        return new Levels(initLevels(levels));
    }

    private static List<Level> initLevels(List<Integer> levels) {
        return levels.stream()
                .map(Level::new)
                .collect(Collectors.toList());
    }

    public String getZones() {
        return levels.stream()
                .map(Level::getZone)
                .map(String::valueOf)
                .collect(Collectors.joining(DELIMITER));
    }
}

class Level {

    private final int level;

    public Level(int level) {
        this.level = level;
    }

    public int getZone() {
        return Zone.from(level).getZone();
    }
}

enum Zone {

    MAX_LEVEL_ZONE(1, level -> level == 300),
    PREVIOUS_MAX_LEVEL_ZONE(2, level -> level >= 275 && level < 300),
    FORMER_MAX_LEVEL_ZONE(3, level -> level >= 250 && level < 275),
    OTHER_LEVEL_ZONE(4, level -> true);

    private final int zone;
    private final Predicate<Integer> matcher;

    Zone(int zone, Predicate<Integer> matcher) {
        this.zone = zone;
        this.matcher = matcher;
    }

    public static Zone from(int level) {
        return Arrays.stream(values())
                .filter(zone -> zone.matcher.test(level))
                .findFirst()
                .orElse(OTHER_LEVEL_ZONE);
    }

    public int getZone() {
        return zone;
    }
}
~~~
