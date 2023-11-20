# Darius님 한타 안 함? (Bronze 4)

### 문제 설명

<img src="https://upload.acmicpc.net/7e511773-be18-4dcc-a516-00dc87dda2f4/-/preview/">

아무래도 우리 팀 다리우스가 고수인 것 같다. 그의 $K/D/A$를 보고 그가 「진짜」인지 판별해 보자.
 

$K+A < D$이거나, $D = 0$이면 그는 「가짜」이고, 그렇지 않으면 「진짜」이다.

출처 : https://www.acmicpc.net/problem/20499

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        KDA kDA = KDA.from(scanner.nextLine());
        System.out.println(kDA.isReal());
    }
}

class KDA {

    private static final String DELIMITER = "/";
    private static final int KILL_POINT_INDEX = 0;
    private static final int DEATH_POINT_INDEX = 1;
    private static final int ASSIST_POINT_INDEX = 2;
    private static final String THE_REAL = "gosu";
    private static final String THE_FAKE = "hasu";

    private final int killPoint;
    private final int deathPoint;
    private final int assistPoint;

    public KDA(int killPoint, int deathPoint, int assistPoint) {
        this.killPoint = killPoint;
        this.deathPoint = deathPoint;
        this.assistPoint = assistPoint;
    }

    public static KDA from(String kDA) {
        String[] splitKDA = kDA.split(DELIMITER);
        int killPoint = Integer.parseInt(splitKDA[KILL_POINT_INDEX]);
        int deathPoint = Integer.parseInt(splitKDA[DEATH_POINT_INDEX]);
        int assistPoint = Integer.parseInt(splitKDA[ASSIST_POINT_INDEX]);
        return new KDA(killPoint, deathPoint, assistPoint);
    }

    public String isReal() {
        return isTheRealKDA() ? THE_REAL : THE_FAKE;
    }

    private boolean isTheRealKDA() {
        return deathPoint != 0 && killPoint + assistPoint >= deathPoint;
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

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideKDA")
    @DisplayName("KDA가 주어지면 '진짜'인지 '가짜'인지 확인한다.")
    void kDAChecksTheRealOrTheFake(String kda, String expected) {
        KDA sut = KDA.from(kda);

        String actual = sut.isReal();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideKDA() {
        return Stream.of(
                Arguments.of("1/1/0", "gosu"),
                Arguments.of("1/0/1", "hasu"),
                Arguments.of("1/5/1", "hasu"),
                Arguments.of("2/4/2", "gosu"),
                Arguments.of("0/5/3", "hasu"),
                Arguments.of("12/4/5", "gosu"),
                Arguments.of("0/0/1", "hasu")
        );
    }
}

class KDA {

    private static final String DELIMITER = "/";
    private static final int KILL_POINT_INDEX = 0;
    private static final int DEATH_POINT_INDEX = 1;
    private static final int ASSIST_POINT_INDEX = 2;
    private static final String THE_REAL = "gosu";
    private static final String THE_FAKE = "hasu";

    private final int killPoint;
    private final int deathPoint;
    private final int assistPoint;

    public KDA(int killPoint, int deathPoint, int assistPoint) {
        this.killPoint = killPoint;
        this.deathPoint = deathPoint;
        this.assistPoint = assistPoint;
    }

    public static KDA from(String kDA) {
        String[] splitKDA = kDA.split(DELIMITER);
        int killPoint = Integer.parseInt(splitKDA[KILL_POINT_INDEX]);
        int deathPoint = Integer.parseInt(splitKDA[DEATH_POINT_INDEX]);
        int assistPoint = Integer.parseInt(splitKDA[ASSIST_POINT_INDEX]);
        return new KDA(killPoint, deathPoint, assistPoint);
    }

    public String isReal() {
        return isTheRealKDA() ? THE_REAL : THE_FAKE;
    }

    private boolean isTheRealKDA() {
        return deathPoint != 0 && killPoint + assistPoint >= deathPoint;
    }
}
~~~
