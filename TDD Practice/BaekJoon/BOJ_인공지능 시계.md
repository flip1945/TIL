# 인공지능 시계 (Bronze 4)

### 문제 설명

KOI 전자에서는 건강에 좋고 맛있는 훈제오리구이 요리를 간편하게 만드는 인공지능 오븐을 개발하려고 한다. 인공지능 오븐을 사용하는 방법은 적당한 양의 오리 훈제 재료를 인공지능 오븐에 넣으면 된다. 그러면 인공지능 오븐은 오븐구이가 끝나는 시간을 초 단위로 자동적으로 계산한다. 

또한, KOI 전자의 인공지능 오븐 앞면에는 사용자에게 훈제오리구이 요리가 끝나는 시각을 알려 주는 디지털 시계가 있다.  

훈제오리구이를 시작하는 시각과 오븐구이를 하는 데 필요한 시간이 초 단위로 주어졌을 때, 오븐구이가 끝나는 시각을 계산하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/2530

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int hour = scanner.nextInt();
        int minute = scanner.nextInt();
        int second = scanner.nextInt();
        int cookingTime = scanner.nextInt();

        AiClock aiClock = new AiClock(hour, minute, second);
        System.out.println(aiClock.getEndTime(cookingTime));
    }
}

class AiClock {

    private static final int SECONDS_OF_DAY = 86400;
    private static final int SECONDS_OF_HOUR = 3600;
    private static final int SECONDS_OF_MINUTE = 60;
    private static final String DELIMITER = " ";

    private final int startHours;
    private final int startMinutes;
    private final int startSeconds;

    public AiClock(int startHours, int startMinutes, int startSeconds) {
        this.startHours = startHours;
        this.startMinutes = startMinutes;
        this.startSeconds = startSeconds;
    }

    public String getEndTime(int cookingTimeOfOven) {
        int totalSecondsOfDay = getTotalSecondsOfDay(cookingTimeOfOven);
        return getHours(totalSecondsOfDay) + DELIMITER + getMinutes(totalSecondsOfDay) + DELIMITER +
                getSeconds(totalSecondsOfDay);
    }

    private int getTotalSecondsOfDay(int cookingTimeOfOven) {
        int totalSecondsOfDay = startHours * SECONDS_OF_HOUR + startMinutes * SECONDS_OF_MINUTE + startSeconds + cookingTimeOfOven;
        if (totalSecondsOfDay >= SECONDS_OF_DAY) {
            return totalSecondsOfDay % SECONDS_OF_DAY;
        }
        return totalSecondsOfDay;
    }

    private int getSeconds(int totalSecondsOfDay) {
        return totalSecondsOfDay % SECONDS_OF_MINUTE;
    }

    private int getMinutes(int totalSecondsOfDay) {
        return totalSecondsOfDay % SECONDS_OF_HOUR / SECONDS_OF_MINUTE;
    }

    private int getHours(int totalSecondsOfDay) {
        return totalSecondsOfDay / SECONDS_OF_HOUR;
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
    @MethodSource("provideStampAndPrice")
    @DisplayName("ai 시계는 오븐이 요리가 완료되는 시간을 계산한다.")
    void aiClockCalculatesOvenEndTime(int hour, int minute, int second, int cookingTimeOfOven, String expected) {
        AiClock sut = new AiClock(hour, minute, second);

        String actual = sut.getEndTime(cookingTimeOfOven);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideStampAndPrice() {
        return Stream.of(
                Arguments.of(0, 0, 0, 0, "0 0 0"),
                Arguments.of(0, 0, 0, 1, "0 0 1"),
                Arguments.of(0, 0, 0, 60, "0 1 0"),
                Arguments.of(0, 0, 0, 61, "0 1 1"),
                Arguments.of(0, 0, 0, 3600, "1 0 0"),
                Arguments.of(0, 0, 10, 51, "0 1 1"),
                Arguments.of(14, 30, 0, 200, "14 33 20"),
                Arguments.of(17, 40, 45, 6015, "19 21 0"),
                Arguments.of(23, 48, 59, 2515, "0 30 54"),
                Arguments.of(23, 59, 59, 86400, "23 59 59"),
                Arguments.of(23, 59, 59, 86401, "0 0 0")
        );
    }
}

class AiClock {

    private static final int SECONDS_OF_DAY = 86400;
    private static final int SECONDS_OF_HOUR = 3600;
    private static final int SECONDS_OF_MINUTE = 60;
    private static final String DELIMITER = " ";

    private final int startHours;
    private final int startMinutes;
    private final int startSeconds;

    public AiClock(int startHours, int startMinutes, int startSeconds) {
        this.startHours = startHours;
        this.startMinutes = startMinutes;
        this.startSeconds = startSeconds;
    }

    public String getEndTime(int cookingTimeOfOven) {
        int totalSecondsOfDay = getTotalSecondsOfDay(cookingTimeOfOven);
        return getHours(totalSecondsOfDay) + DELIMITER + getMinutes(totalSecondsOfDay) + DELIMITER +
                getSeconds(totalSecondsOfDay);
    }

    private int getTotalSecondsOfDay(int cookingTimeOfOven) {
        int totalSecondsOfDay = startHours * SECONDS_OF_HOUR + startMinutes * SECONDS_OF_MINUTE + startSeconds
                + cookingTimeOfOven;
        if (totalSecondsOfDay >= SECONDS_OF_DAY) {
            return totalSecondsOfDay % SECONDS_OF_DAY;
        }
        return totalSecondsOfDay;
    }

    private int getHours(int totalSecondsOfDay) {
        return totalSecondsOfDay / SECONDS_OF_HOUR;
    }

    private int getMinutes(int totalSecondsOfDay) {
        return totalSecondsOfDay % SECONDS_OF_HOUR / SECONDS_OF_MINUTE;
    }

    private int getSeconds(int totalSecondsOfDay) {
        return totalSecondsOfDay % SECONDS_OF_MINUTE;
    }
}
~~~
