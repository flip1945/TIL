# Atrium (Bronze 4)

### 문제 설명

The atrium of a traditional Roman dormus, much like the atria of today, is a perfectly square room designed for residents and guests to congregate in and to enjoy the sunlight streaming in from above. Or, in the case of Britannia, the rain streaming in from above.

A major problem with traditional Roman architecture, particularly in modern times, is the absence of any kind of effective walls between rooms. You have arrived in Italy and now you are going to helpfully rebuild the walls on behalf of the authorities, starting with the atrium of a particularly derelict building you found.

What length of prefabricated wall section must you bring with you to fully enclose the atrium of the building?

출처 : https://www.acmicpc.net/problem/20353

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        long area = scanner.nextLong();
        Atrium atrium = new Atrium(area);
        System.out.println(atrium.perimeter());
    }
}

class Atrium {

    private static final int COUNT_OF_SIDES = 4;

    private final long area;

    public Atrium(long area) {
        this.area = area;
    }

    public double perimeter() {
        return getSide() * COUNT_OF_SIDES;
    }

    private double getSide() {
        return Math.sqrt(area);
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

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideArea")
    @DisplayName("면적이 주어지면 아트리움의 둘레를 반환한다.")
    void atriumReturnsPerimeterGivenArea(long area, double expected) {
        Atrium sut = new Atrium(area);

        double actual = sut.perimeter();

        assertEquals(expected, actual, 0.000001);
    }

    static Stream<Arguments> provideArea() {
        return Stream.of(
                Arguments.of(1, 4.0),
                Arguments.of(16, 16.0),
                Arguments.of(64, 32.0),
                Arguments.of(1234, 140.51334456),
                Arguments.of(1000000000000000000L, 4000000000L)
        );
    }
}

class Atrium {

    private static final int COUNT_OF_SIDES = 4;

    private final long area;

    public Atrium(long area) {
        this.area = area;
    }

    public double perimeter() {
        return getSide() * COUNT_OF_SIDES;
    }

    private double getSide() {
        return Math.sqrt(area);
    }
}
~~~
