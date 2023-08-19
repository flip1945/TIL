# 미국 스타일 (Bronze 3)

### 문제 설명

싸이가 강남 스타일로 2012년 10월 4일 현재 빌보드 핫100 차트 2위에 2주 연속 랭크되고 있다. 싸이는 곧 다시 미국으로 가서 해외 활동할 예정이라고 한다.

하지만 미국은 한국과 사용하는 단위 체계가 다르다. 한국은 미터법을 사용하지만, 미국은 미국 단위계를 사용한다. 싸이를 위해 단위를 바꾸어 주는 프로그램을 작성하시오.

아래 표를 참고해서 계산하면 되고, 킬로그램 <-> 파운드, 리터 <-> 갤런만 변환하면 된다.

|종류|	미터법|	미국 단위계|
|-|-|-|
|무게|	1.000 킬로그램|	2.2046 파운드|
| 	|0.4536 킬로그램|	1.0000 파운드|
|부피|	1.0000 리터|	0.2642 갤런|
| 	|3.7854 리터|	1.0000 갤런|

출처 : https://www.acmicpc.net/problem/2712

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            UnitConverter converter = new UnitConverter();
            System.out.println(converter.convert(scanner.nextLine()));
        }
    }
}

class UnitConverter {

    private static final String DELIMITER = " ";

    private static final int VALUE_INDEX = 0;
    private static final int UNIT_INDEX = 1;

    public String convert(String unit) {
        double value = Double.parseDouble(unit.split(DELIMITER)[VALUE_INDEX]);
        Unit fromUnit = Unit.from(unit.split(DELIMITER)[UNIT_INDEX]);
        return fromUnit.convert(value);
    }
}

enum Unit {
    KILOGRAM("kg", value -> String.format("%.4f lb", value * 2.2046)),
    POUND("lb", value -> String.format("%.4f kg", value * 0.4536)),
    LITER("l", value -> String.format("%.4f g", value * 0.2642)),
    GALLON("g", value -> String.format("%.4f l", value * 3.7854));

    private final String unit;
    private final Function<Double, String> converter;

    Unit(String unit, Function<Double, String> converter) {
        this.unit = unit;
        this.converter = converter;
    }

    public static Unit from(String unit) {
        for (Unit value : values()) {
            if (value.unit.equals(unit)) {
                return value;
            }
        }
        throw new IllegalArgumentException();
    }

    public String convert(double value) {
        return converter.apply(value);
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

import java.util.function.Function;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideUnits")
    @DisplayName("단위(kg, lb, l, g)가 주어졌을 때 다른 단위로 변환해야 한다.")
    void shouldConvertUnit(String unit, String expected) {
        UnitConverter sut = new UnitConverter();

        String actual = sut.convert(unit);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideUnits() {
        return Stream.of(
                Arguments.of("1 kg", "2.2046 lb"),
                Arguments.of("2 kg", "4.4092 lb"),
                Arguments.of("0.4536 kg", "1.0000 lb"),
                Arguments.of("0.9072 kg", "2.0000 lb"),
                Arguments.of("1 lb", "0.4536 kg"),
                Arguments.of("2 lb", "0.9072 kg"),
                Arguments.of("2.2046 lb", "1.0000 kg"),
                Arguments.of("4.4092 lb", "2.0000 kg"),
                Arguments.of("1 l", "0.2642 g"),
                Arguments.of("2 l", "0.5284 g"),
                Arguments.of("1 g", "3.7854 l"),
                Arguments.of("2 g", "7.5708 l"),
                Arguments.of("7 lb", "3.1752 kg"),
                Arguments.of("3.5 g", "13.2489 l"),
                Arguments.of("0 l", "0.0000 g")
        );
    }
}

class UnitConverter {

    private static final String DELIMITER = " ";

    private static final int VALUE_INDEX = 0;
    private static final int UNIT_INDEX = 1;

    public String convert(String unit) {
        double value = Double.parseDouble(unit.split(DELIMITER)[VALUE_INDEX]);
        Unit fromUnit = Unit.from(unit.split(DELIMITER)[UNIT_INDEX]);
        return fromUnit.convert(value);
    }
}

enum Unit {
    KILOGRAM("kg", value -> String.format("%.4f lb", value * 2.2046)),
    POUND("lb", value -> String.format("%.4f kg", value * 0.4536)),
    LITER("l", value -> String.format("%.4f g", value * 0.2642)),
    GALLON("g", value -> String.format("%.4f l", value * 3.7854));

    private final String unit;
    private final Function<Double, String> converter;

    Unit(String unit, Function<Double, String> converter) {
        this.unit = unit;
        this.converter = converter;
    }

    public static Unit from(String unit) {
        for (Unit value : values()) {
            if (value.unit.equals(unit)) {
                return value;
            }
        }
        throw new IllegalArgumentException();
    }

    public String convert(double value) {
        return converter.apply(value);
    }
}
~~~
