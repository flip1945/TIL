# 간지(干支) (Bronze 2)

### 문제 설명

예로부터 동아시아에서는 십간(十干)과 십이지(十二支)를 사용하여 연도를 표시하였다. 십간은 "갑을병정무기경신임계"를 말하며 십이지는 "자축인묘진사오미신유술해"를 말한다. 십간과 십이지로 구성된 간지를 사용하여 60년을 주기로 각 연도에 다음과 같이 이름을 부여한다: 최초 1년째는 "갑자"이고, 2년째는 "을축", 3년째는 "병인" 과 같이 올해의 간지에서 십간과 십이지의 다음 문자를 이듬해의 간지로 사용한다. 십간은 10년을 주기로, 십이지는 12년을 주기로 순환된다. 이런 순서로 하여 마지막 "계해"는 60년째를 나타내고, 61년째는 다시 "갑자"가 된다.

60갑자를 서양식으로 나타내기 위해 

1. 십간을 0부터 9까지의 정수로 표현하고 
2. 십이지를 "ABCDEFGHIJKL"로 표현하고
3. 십간과 십이지의 순서를 바꾼다고 하자.

이를 서양식 간지 표현법이라고 부르자. 예를 들면, "갑자"는 "A0"로 "을축"은 "B1", "계해"는 "L9"으로 표현된다. 2013년은 계사년이므로 "F9"으로 표현되고, 2014년은 갑오년으로 "G0" 로 표현된다.

입력으로 주어진 연도를 서양식 간지 표현법으로 나타낸 것을 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/7572

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        SexagenaryCycle sexagenaryCycle = new SexagenaryCycle(new EarthlyBranches(), new HeavenStems());
        System.out.println(sexagenaryCycle.getSexagenaryCycle(scanner.nextInt()));
    }
}

class SexagenaryCycle {

    private final EarthlyBranches earthlyBranches;
    private final HeavenStems heavenStems;

    public SexagenaryCycle(EarthlyBranches earthlyBranches, HeavenStems heavenStems) {
        this.earthlyBranches = earthlyBranches;
        this.heavenStems = heavenStems;
    }

    public String getSexagenaryCycle(int year) {
        return earthlyBranches.getEarthlyBranches(year) + heavenStems.getHeavenStems(year);
    }
}

class EarthlyBranches {

    private static final String[] earthlyBranches = new String[]{"A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L"};
    private static final int EARTHLY_BRANCHES_SIZE = 12;
    private static final int EARTHLY_BRANCHES_PADDING = 8;

    public String getEarthlyBranches(int year) {
        return earthlyBranches[(year + EARTHLY_BRANCHES_PADDING) % EARTHLY_BRANCHES_SIZE];
    }
}

class HeavenStems {

    private static final int HEAVEN_STEMS_SIZE = 10;
    private static final int HEAVEN_STEMS_PADDING = 6;

    public String getHeavenStems(int year) {
        return String.valueOf((year + HEAVEN_STEMS_PADDING) % HEAVEN_STEMS_SIZE);
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
    @MethodSource("provideYears")
    @DisplayName("주어진 년도의 간지를 반환한다.")
    void sexagenaryCycleTest(int year, String expected) {
        SexagenaryCycle sut = new SexagenaryCycle(new EarthlyBranches(), new HeavenStems());

        String actual = sut.getSexagenaryCycle(year);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideYears() {
        return Stream.of(
                Arguments.of(4, "A0"),
                Arguments.of(5, "B1"),
                Arguments.of(63, "L9"),
                Arguments.of(64, "A0"),
                Arguments.of(2013, "F9"),
                Arguments.of(2060, "E6")
        );
    }
}

class SexagenaryCycle {

    private final EarthlyBranches earthlyBranches;
    private final HeavenStems heavenStems;

    public SexagenaryCycle(EarthlyBranches earthlyBranches, HeavenStems heavenStems) {
        this.earthlyBranches = earthlyBranches;
        this.heavenStems = heavenStems;
    }

    public String getSexagenaryCycle(int year) {
        return earthlyBranches.getEarthlyBranches(year) + heavenStems.getHeavenStems(year);
    }
}

class EarthlyBranches {

    private static final String[] earthlyBranches = new String[]{"A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L"};
    private static final int EARTHLY_BRANCHES_SIZE = 12;
    private static final int EARTHLY_BRANCHES_PADDING = 8;

    public String getEarthlyBranches(int year) {
        return earthlyBranches[(year + EARTHLY_BRANCHES_PADDING) % EARTHLY_BRANCHES_SIZE];
    }
}

class HeavenStems {

    private static final int HEAVEN_STEMS_SIZE = 10;
    private static final int HEAVEN_STEMS_PADDING = 6;

    public String getHeavenStems(int year) {
        return String.valueOf((year + HEAVEN_STEMS_PADDING) % HEAVEN_STEMS_SIZE);
    }
}
~~~
