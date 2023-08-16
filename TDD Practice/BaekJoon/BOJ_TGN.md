# TGN (Bronze 3)

### 문제 설명

상근이는 TGN사의 사장이다. TGN은 Teenager Game Network의 약자 같지만, 사실 Temporary Group Name의 약자이다.

이 회사는 청소년을 위한 앱을 만드는 회사이다. 일년에 걸친 개발기간 끝에 드디어 앱을 완성했고, 이제 팔기만 하면 된다.

상근이는 데이트를 인간의 두뇌로 이해할 수 없을 정도로 많이 한다. 따라서 엄청난 데이트 비용이 필요하다. 상근이는 광고를 적절히 해서 수익을 최대한 올리려고 한다.

어느 날 하늘을 바라보던 상근이는 시리우스의 기운을 받게 되었고, 광고 효과를 예측하는 능력을 갖게 되었다.

광고 효과가 주어졌을 때, 광고를 해야할지 말아야할지 결정하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/5063

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            Advertise advertise = Advertise.from(scanner.nextLine());
            System.out.println(advertise.determine());
        }
    }
}

class Advertise {

    public static final String DELIMITER = " ";
    public static final int EARNINGS_WITHOUT_AD_INDEX = 0;
    public static final int EARNINGS_WITH_AD_INDEX = 1;
    public static final int AD_COSTS_INDEX = 2;
    public static final String DOES_NOT_MATTER = "does not matter";
    public static final String DO_ADVERTISE = "advertise";
    public static final String DONT_ADVERTISE = "do not advertise";

    private final int earningsWithoutAd;
    private final int earningsWithAd;
    private final int adCosts;

    private Advertise(int earningsWithoutAd, int earningsWithAd, int adCosts) {
        this.earningsWithoutAd = earningsWithoutAd;
        this.earningsWithAd = earningsWithAd;
        this.adCosts = adCosts;
    }

    public static Advertise from(String advertiseInfo) {
        String[] splitString = advertiseInfo.split(DELIMITER);
        return new Advertise(Integer.parseInt(splitString[EARNINGS_WITHOUT_AD_INDEX]),
                Integer.parseInt(splitString[EARNINGS_WITH_AD_INDEX]), Integer.parseInt(splitString[AD_COSTS_INDEX]));
    }

    public String determine() {
        int earningsWithAdSpend = earningsWithAd - adCosts;
        if (earningsWithAdSpend == earningsWithoutAd) {
            return DOES_NOT_MATTER;
        }
        return earningsWithAdSpend > earningsWithoutAd ? DO_ADVERTISE : DONT_ADVERTISE;
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
    @MethodSource("provideAdvertiseInfos")
    @DisplayName("광고 비용 정보가 주어졌을 때 광고를 해야할지 말아야 할 지 상관없을 지 판단한다.")
    void determineWhetherItShouldBeAdvertisedOrNot(String advertiseInfo, String expected) {
        Advertise sut = Advertise.from(advertiseInfo);

        String actual = sut.determine();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideAdvertiseInfos() {
        return Stream.of(
                Arguments.of("0 10 0", "advertise"),
                Arguments.of("10 5 0", "do not advertise"),
                Arguments.of("10 10 0", "does not matter"),
                Arguments.of("10 10 5", "do not advertise"),
                Arguments.of("0 100 70", "advertise"),
                Arguments.of("100 130 30", "does not matter"),
                Arguments.of("-100 -70 40", "do not advertise")
        );
    }
}

class Advertise {

    public static final String DELIMITER = " ";
    public static final int EARNINGS_WITHOUT_AD_INDEX = 0;
    public static final int EARNINGS_WITH_AD_INDEX = 1;
    public static final int AD_COSTS_INDEX = 2;
    public static final String DOES_NOT_MATTER = "does not matter";
    public static final String DO_ADVERTISE = "advertise";
    public static final String DONT_ADVERTISE = "do not advertise";

    private final int earningsWithoutAd;
    private final int earningsWithAd;
    private final int adCosts;

    private Advertise(int earningsWithoutAd, int earningsWithAd, int adCosts) {
        this.earningsWithoutAd = earningsWithoutAd;
        this.earningsWithAd = earningsWithAd;
        this.adCosts = adCosts;
    }

    public static Advertise from(String advertiseInfo) {
        String[] splitString = advertiseInfo.split(DELIMITER);
        return new Advertise(Integer.parseInt(splitString[EARNINGS_WITHOUT_AD_INDEX]),
                Integer.parseInt(splitString[EARNINGS_WITH_AD_INDEX]), Integer.parseInt(splitString[AD_COSTS_INDEX]));
    }

    public String determine() {
        int earningsWithAdSpend = earningsWithAd - adCosts;
        if (earningsWithAdSpend == earningsWithoutAd) {
            return DOES_NOT_MATTER;
        }
        return earningsWithAdSpend > earningsWithoutAd ? DO_ADVERTISE : DONT_ADVERTISE;
    }
}
~~~
