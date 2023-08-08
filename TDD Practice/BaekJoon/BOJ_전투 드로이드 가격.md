# 전투 드로이드 가격 (Bronze 3)

### 문제 설명

상근이는 망가진 전투 드로이드를 고치려고 하고 있다. 전투 드로이드의 각 부품의 가격은 다음과 같다.

|부품|가격|
|-|-|
|블래스터 라이플|	$350.34|
|시각 센서|	$230.90|
|청각 센서|	$190.55|
|팔|	$125.30|
|다리|	$180.90|

출처 : https://www.acmicpc.net/problem/5361

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            BattleDroid battleDroid = BattleDroid.from(scanner.nextLine());
            System.out.println(battleDroid.getTotalExpenses());
        }
    }
}

class BattleDroid {

    public static final String DELIMITER = " ";
    public static final int BLAST_RIFLE_INDEX = 0;
    public static final int VISUAL_SENSOR_INDEX = 1;
    public static final int AUDITORY_SENSOR_INDEX = 2;
    public static final int ARM_INDEX = 3;
    public static final int LEG_INDEX = 4;
    public static final String CURRENCY_SYMBOL = "$";
    public static final String CURRENCY_FORMAT = "%.2f";

    private final int countOfBlastRifle;
    private final int countOfVisualSensors;
    private final int countOfAuditorySensors;
    private final int countOfArms;
    private final int countOfLegs;

    private BattleDroid(int countOfBlastRifle, int countOfVisualSensors, int countOfAuditorySensors, int countOfArms,
                        int countOfLegs) {
        this.countOfBlastRifle = countOfBlastRifle;
        this.countOfVisualSensors = countOfVisualSensors;
        this.countOfAuditorySensors = countOfAuditorySensors;
        this.countOfArms = countOfArms;
        this.countOfLegs = countOfLegs;
    }

    public static BattleDroid from(String parts) {
        String[] split = parts.split(DELIMITER);
        return new BattleDroid(Integer.parseInt(split[BLAST_RIFLE_INDEX]),
                Integer.parseInt(split[VISUAL_SENSOR_INDEX]),
                Integer.parseInt(split[AUDITORY_SENSOR_INDEX]),
                Integer.parseInt(split[ARM_INDEX]),
                Integer.parseInt(split[LEG_INDEX]));
    }

    public String getTotalExpenses() {
        return CURRENCY_SYMBOL + String.format(CURRENCY_FORMAT, getTotalPrice());
    }

    private double getTotalPrice() {
        return countOfBlastRifle * Parts.BLAST_RIFLE.getPrice() +
                countOfVisualSensors * Parts.VISUAL_SENSOR.getPrice() +
                countOfAuditorySensors * Parts.AUDITORY_SENSOR.getPrice() +
                countOfArms * Parts.ARM.getPrice() +
                countOfLegs * Parts.LEG.getPrice();
    }
}

enum Parts {
    BLAST_RIFLE(350.34),
    VISUAL_SENSOR(230.90),
    AUDITORY_SENSOR(190.55),
    ARM(125.30),
    LEG(180.90);

    private final double price;

    Parts(double price) {
        this.price = price;
    }

    public double getPrice() {
        return price;
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

import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideParts")
    @DisplayName("전투 드로이드의 총 가격을 반환한다.")
    void testGetBattleDroidExpenses(String parts, String expected) {
        BattleDroid sut = BattleDroid.from(parts);

        String actual = sut.getTotalExpenses();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideParts() {
        return Stream.of(
                Arguments.of("1 0 0 0 0", "$350.34"),
                Arguments.of("2 0 0 0 0", "$700.68"),
                Arguments.of("3 0 0 0 0", "$1051.02"),
                Arguments.of("1 1 0 0 0", "$581.24"),
                Arguments.of("1 2 0 0 0", "$812.14"),
                Arguments.of("1 2 1 0 0", "$1002.69"),
                Arguments.of("1 2 1 1 1", "$1308.89"),
                Arguments.of("20 10 14 3 9", "$13987.50"),
                Arguments.of("19 17 12 8 10", "$15679.76"),
                Arguments.of("11 9 8 22 33", "$16182.54")
        );
    }
}

class BattleDroid {

    public static final String DELIMITER = " ";
    public static final int BLAST_RIFLE_INDEX = 0;
    public static final int VISUAL_SENSOR_INDEX = 1;
    public static final int AUDITORY_SENSOR_INDEX = 2;
    public static final int ARM_INDEX = 3;
    public static final int LEG_INDEX = 4;
    public static final String CURRENCY_SYMBOL = "$";
    public static final String CURRENCY_FORMAT = "%.2f";

    private final int countOfBlastRifle;
    private final int countOfVisualSensors;
    private final int countOfAuditorySensors;
    private final int countOfArms;
    private final int countOfLegs;

    private BattleDroid(int countOfBlastRifle, int countOfVisualSensors, int countOfAuditorySensors, int countOfArms,
                        int countOfLegs) {
        this.countOfBlastRifle = countOfBlastRifle;
        this.countOfVisualSensors = countOfVisualSensors;
        this.countOfAuditorySensors = countOfAuditorySensors;
        this.countOfArms = countOfArms;
        this.countOfLegs = countOfLegs;
    }

    public static BattleDroid from(String parts) {
        String[] split = parts.split(DELIMITER);
        return new BattleDroid(Integer.parseInt(split[BLAST_RIFLE_INDEX]),
                Integer.parseInt(split[VISUAL_SENSOR_INDEX]),
                Integer.parseInt(split[AUDITORY_SENSOR_INDEX]),
                Integer.parseInt(split[ARM_INDEX]),
                Integer.parseInt(split[LEG_INDEX]));
    }

    public String getTotalExpenses() {
        return CURRENCY_SYMBOL + String.format(CURRENCY_FORMAT, getTotalPrice());
    }

    private double getTotalPrice() {
        return countOfBlastRifle * Parts.BLAST_RIFLE.getPrice() +
                countOfVisualSensors * Parts.VISUAL_SENSOR.getPrice() +
                countOfAuditorySensors * Parts.AUDITORY_SENSOR.getPrice() +
                countOfArms * Parts.ARM.getPrice() +
                countOfLegs * Parts.LEG.getPrice();
    }
}

enum Parts {
    BLAST_RIFLE(350.34),
    VISUAL_SENSOR(230.90),
    AUDITORY_SENSOR(190.55),
    ARM(125.30),
    LEG(180.90);

    private final double price;

    Parts(double price) {
        this.price = price;
    }

    public double getPrice() {
        return price;
    }
}
~~~
