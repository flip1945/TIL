# 이 별은 무슨 색일까 (Bronze 5)

### 문제 설명

스타는 안에 별이 담긴 기계장치를 보았다. 기계장치 내부를 볼 수 없어 별을 구경할 순 없었지만, 기계장치에는 별빛의 파장을 알려주는 계기판이 있었다. 계기판에 표시된 파장의 값을 토대로 스타는 별의 색을 알아낼 수 있었다. 스타가 알아낸 별의 색은 무엇이었을까?

색상별 파장의 범위는 다음과 같다.

* 빨간색: 620nm 이상 780nm 이하
* 주황색: 590nm 이상 620nm 미만
* 노란색: 570nm 이상 590nm 미만
* 초록색: 495nm 이상 570nm 미만
* 파란색: 450nm 이상 495nm 미만
* 남색: 425nm 이상 450nm 미만
* 보라색: 380nm 이상 425nm 미만

출처 : https://www.acmicpc.net/problem/30676

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Stream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int waveLength = scanner.nextInt();
        Star star = new Star(waveLength);
        System.out.println(star.color());
    }
}

class Star {
    private final int waveLength;

    public Star(int waveLength) {
        this.waveLength = waveLength;
    }

    public String color() {
        return Color.from(waveLength).getColorName();
    }
}

enum Color {
    VIOLET("Violet", 380, 425),
    INDIGO("Indigo", 425, 450),
    BLUE("Blue", 450, 495),
    GREEN("Green", 495, 570),
    YELLOW("Yellow", 570, 590),
    ORANGE("Orange", 590, 620),
    RED("Red", 620, 781);

    private final String colorName;
    private final int minWaveLength;
    private final int maxWaveLength;

    Color(String colorName, int minWaveLength, int maxWaveLength) {
        this.colorName = colorName;
        this.minWaveLength = minWaveLength;
        this.maxWaveLength = maxWaveLength;
    }

    public static Color from(int waveLength) {
        return Stream.of(values())
            .filter(c -> c.minWaveLength <= waveLength && waveLength < c.maxWaveLength)
            .findFirst()
            .orElseThrow(IllegalArgumentException::new);
    }

    public String getColorName() {
        return colorName;
    }
}
~~~

---

#### 테스트 코드
~~~java
import static org.junit.jupiter.api.Assertions.*;

import java.util.stream.Stream;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideWaveLength")
    @DisplayName("파장이 주어지면 별의 색을 반환한다.")
    void starReturnsStarColorGivenWaveLength(int waveLength, String expected) {
        Star sut = new Star(waveLength);

        String actual = sut.color();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideWaveLength() {
        return Stream.of(
            Arguments.of(380, "Violet"),
            Arguments.of(424, "Violet"),
            Arguments.of(425, "Indigo"),
            Arguments.of(449, "Indigo"),
            Arguments.of(450, "Blue"),
            Arguments.of(494, "Blue"),
            Arguments.of(495, "Green"),
            Arguments.of(569, "Green"),
            Arguments.of(570, "Yellow"),
            Arguments.of(589, "Yellow"),
            Arguments.of(590, "Orange"),
            Arguments.of(619, "Orange"),
            Arguments.of(620, "Red"),
            Arguments.of(780, "Red")
            
        );
    }
}

class Star {
    private final int waveLength;

    public Star(int waveLength) {
        this.waveLength = waveLength;
    }

    public String color() {
        return Color.from(waveLength).getColorName();
    }
}

enum Color {
    VIOLET("Violet", 380, 425),
    INDIGO("Indigo", 425, 450),
    BLUE("Blue", 450, 495),
    GREEN("Green", 495, 570),
    YELLOW("Yellow", 570, 590),
    ORANGE("Orange", 590, 620),
    RED("Red", 620, 781);

    private final String colorName;
    private final int minWaveLength;
    private final int maxWaveLength;
    
    Color(String colorName, int minWaveLength, int maxWaveLength) {
        this.colorName = colorName;
        this.minWaveLength = minWaveLength;
        this.maxWaveLength = maxWaveLength;
    }

    public static Color from(int waveLength) {
        return Stream.of(values())
            .filter(c -> c.minWaveLength <= waveLength && waveLength < c.maxWaveLength)
            .findFirst()
            .orElseThrow(IllegalArgumentException::new);
    }
    
    public String getColorName() {
        return colorName;
    }
}
~~~
