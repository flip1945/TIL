# 합금 주화 (Bronze 1)

### 문제 설명

제1회 AGCU컵의 상품 중에는 은화도 준비되어 있다.

비행씨는 기념주화를 수집하는 취미가 있다. 통장을 탈탈 털어 산 기념주화를 하루 종일 손질하던 비행씨는, 물질의 질량을 부피로 나눈 값인 밀도를 기준으로 기념주화들을 나열하기로 했다.

비행씨는 저울을 갖고 있지 않아, 이미 알고 있는 정보만을 사용해 기념주화의 밀도를 가늠해야 한다. 비행씨가 알고 있는 것은 이 기념주화가 두 금속의 합금으로 되어 있다는 것, 그 두 금속의 섞이기 전 밀도, 그리고 합금을 이루는 두 금속의 질량 비율이다.

물질을 혼합한 후의 부피는 혼합 전 두 물질의 부피의 합이고, 물질을 혼합한 후의 질량도 혼합 전 두 물질의 질량의 합이다.

비행씨에게 기념주화의 밀도를 알려주도록 하자.

출처 : https://www.acmicpc.net/problem/27963

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int density1 = scanner.nextInt();
        int density2 = scanner.nextInt();
        int massRatioOfHigherDensity = scanner.nextInt();

        AlloyCoin alloyCoin = new AlloyCoin(density1, density2, massRatioOfHigherDensity);
        System.out.println(alloyCoin.getTotalDensity());
    }
}

class AlloyCoin {

    public static final int TOTAL_MASS = 100;
    private final Coin firstCoin;
    private final Coin secondCoin;

    public AlloyCoin(int density1, int density2, int massRatioOfHigherDensity) {
        this.firstCoin = Coin.of(TOTAL_MASS - massRatioOfHigherDensity, getLowerDensity(density1, density2));
        this.secondCoin = Coin.of(massRatioOfHigherDensity, getHigherDensity(density1, density2));
    }

    private double getLowerDensity(int density1, int density2) {
        return Math.min(density1, density2);
    }

    private double getHigherDensity(int density1, int density2) {
        return Math.max(density1, density2);
    }

    public double getTotalDensity() {
        return TOTAL_MASS / (firstCoin.getVolume() + secondCoin.getVolume());
    }
}

class Coin {

    private final double mass;
    private final double density;
    private final double volume;

    private Coin(double mass, double density) {
        this.mass = mass;
        this.density = density;
        this.volume = calculateVolume();
    }

    private double calculateVolume() {
        return mass / density;
    }

    public static Coin of(double density, double mass) {
        return new Coin(density, mass);
    }

    public double getVolume() {
        return volume;
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
    @MethodSource("provideCoinDensityAndMassRatio")
    @DisplayName("합금 주화의 통합 밀도를 반환한다.")
    void getTotalDensityTest(int density1, int density2, int massRatioOfHigherDensity, double expected) {
        AlloyCoin sut = new AlloyCoin(density1, density2, massRatioOfHigherDensity);

        double actual = sut.getTotalDensity();

        assertEquals(expected, actual, 0.000001);
    }

    private static Stream<Arguments> provideCoinDensityAndMassRatio() {
        return Stream.of(
                Arguments.of(8, 10, 50, 8.88888888f),
                Arguments.of(8, 2, 80, 5.0f),
                Arguments.of(1, 99, 50, 1.98f),
                Arguments.of(1, 4, 80, 2.5f)
        );
    }
}

class AlloyCoin {

    public static final int TOTAL_MASS = 100;
    private final Coin firstCoin;
    private final Coin secondCoin;

    public AlloyCoin(int density1, int density2, int massRatioOfHigherDensity) {
        this.firstCoin = Coin.of(TOTAL_MASS - massRatioOfHigherDensity, getLowerDensity(density1, density2));
        this.secondCoin = Coin.of(massRatioOfHigherDensity, getHigherDensity(density1, density2));
    }

    private double getLowerDensity(int density1, int density2) {
        return Math.min(density1, density2);
    }

    private double getHigherDensity(int density1, int density2) {
        return Math.max(density1, density2);
    }

    public double getTotalDensity() {
        return TOTAL_MASS / (firstCoin.getVolume() + secondCoin.getVolume());
    }
}

class Coin {

    private final double mass;
    private final double density;
    private final double volume;

    private Coin(double mass, double density) {
        this.mass = mass;
        this.density = density;
        this.volume = calculateVolume();
    }

    private double calculateVolume() {
        return mass / density;
    }

    public static Coin of(double density, double mass) {
        return new Coin(density, mass);
    }

    public double getVolume() {
        return volume;
    }
}
~~~
