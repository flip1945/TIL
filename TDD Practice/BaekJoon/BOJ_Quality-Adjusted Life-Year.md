# Quality-Adjusted Life-Year (Bronze 3)

### 문제 설명

The Quality-Adjusted Life-Year (QALY) is a way to measure a person's quality of life that includes both the quality and the quantity of life lived.

The quality of life lived can be quantified as a number between 
$0$ and 
$1$.  If someone is living with perfect health, the quality of life is 
$1$.  If someone is dead, then the quality of life is 
$0$.  The quality of life may increase or decrease due to medical treatements, sickness, etc.

The QALY for each period in which the quality of life is constant is simply the product of the quality of life and the length of the period (in years).  We wish to know the amount of QALY accumulated by a person at the time of death, given the complete history of this person.

출처 : https://www.acmicpc.net/problem/22279

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        LifeQualities lifeQualities = LifeQualities.from(inputLifeQualities(scanner, n));
        System.out.println(lifeQualities.calculate());
    }

    private static List<String> inputLifeQualities(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class LifeQualities {

    private final List<LifeQuality> lifeQualities;

    private LifeQualities(List<LifeQuality> lifeQualities) {
        this.lifeQualities = lifeQualities;
    }

    public static LifeQualities from(List<String> lifeQualities) {
        return new LifeQualities(initLifeQualities(lifeQualities));
    }

    private static List<LifeQuality> initLifeQualities(List<String> lifeQualities) {
        return lifeQualities.stream().map(LifeQuality::from).collect(Collectors.toList());
    }

    public double calculate() {
        return lifeQualities.stream()
                .mapToDouble(LifeQuality::calculate)
                .sum();
    }
}

class LifeQuality {

    public static final String DELIMITER = " ";
    public static final int QUALITY_INDEX = 0;
    public static final int YEARS_INDEX = 1;

    private final double quality;
    private final double years;

    private LifeQuality(double quality, double years) {
        this.quality = quality;
        this.years = years;
    }

    public static LifeQuality from(String lifeQuality) {
        String[] split = lifeQuality.split(DELIMITER);
        return new LifeQuality(Double.parseDouble(split[QUALITY_INDEX]), Double.parseDouble(split[YEARS_INDEX]));
    }

    public double calculate() {
        return quality * years;
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
    @MethodSource("provideLifeQualities")
    @DisplayName("삶의 질의 점수를 계산한다.")
    void calculateLifeQualityTest(List<String> lifeQualities, double expected) {
        LifeQualities sut = LifeQualities.from(lifeQualities);

        double actual = sut.calculate();

        assertEquals(expected, actual, 0.001);
    }

    private static Stream<Arguments> provideLifeQualities() {
        return Stream.of(
                Arguments.of(List.of("1.0 1.0"), 1.0),
                Arguments.of(List.of("1.0 2.0"), 2.0),
                Arguments.of(List.of("1.0 2.0", "2.0 2.0"), 6.0),
                Arguments.of(List.of("1.0 12.0", "0.7 5.2", "0.9 10.7", "0.5 20.4", "0.2 30.0"), 41.470)
        );
    }
}

class LifeQualities {

    private final List<LifeQuality> lifeQualities;

    private LifeQualities(List<LifeQuality> lifeQualities) {
        this.lifeQualities = lifeQualities;
    }

    public static LifeQualities from(List<String> lifeQualities) {
        return new LifeQualities(initLifeQualities(lifeQualities));
    }

    private static List<LifeQuality> initLifeQualities(List<String> lifeQualities) {
        return lifeQualities.stream().map(LifeQuality::from).collect(Collectors.toList());
    }

    public double calculate() {
        return lifeQualities.stream()
                .mapToDouble(LifeQuality::calculate)
                .sum();
    }
}

class LifeQuality {

    public static final String DELIMITER = " ";
    public static final int QUALITY_INDEX = 0;
    public static final int YEARS_INDEX = 1;

    private final double quality;
    private final double years;

    private LifeQuality(double quality, double years) {
        this.quality = quality;
        this.years = years;
    }

    public static LifeQuality from(String lifeQuality) {
        String[] split = lifeQuality.split(DELIMITER);
        return new LifeQuality(Double.parseDouble(split[QUALITY_INDEX]), Double.parseDouble(split[YEARS_INDEX]));
    }

    public double calculate() {
        return quality * years;
    }
}
~~~
