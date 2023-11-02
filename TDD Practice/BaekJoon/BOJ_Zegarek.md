# Zegarek (Bronze 3)

### 문제 설명

Bajtazar właśnie pracuje nad oprogramowaniem dla nowego, bardzo zaawansowanego zegarka. Jedną z jego funkcji ma być wyświetlanie bieżącej godziny. Co sekundę wskazanie wyświetlacza ma się zmienić na aktualne: na przykład, jeśli zegarek aktualnie pokazuje 17:08:50 to po upływie sekundy powinien pokazać 17:08:51. Bajtazar chce sprawdzić czy zegarek prawidłowo zmienia wskazania. Możesz mu w tym pomóc?

Napisz program, który: wczyta bieżące wskazanie zegarka, wyliczy jakie powinno być wskazanie za sekundę i wypisze wynik na standardowe wyjście.

출처 : https://www.acmicpc.net/problem/26752

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String timeString = scanner.nextLine();

        Time time = Time.from(timeString);
        System.out.println(time.next());
    }
}

class Time {

    private static final String DELIMITER = " ";
    private static final int HOUR_INDEX = 0;
    private static final int MINUTE_INDEX = 1;
    private static final int SECOND_INDEX = 2;

    private final Hour hour;
    private final Minute minute;
    private final Second second;

    public Time(Hour hour, Minute minute, Second second) {
        this.hour = hour;
        this.minute = minute;
        this.second = second;
    }

    public static Time from(String timeString) {
        String[] split = timeString.split(DELIMITER);
        int hour = Integer.parseInt(split[HOUR_INDEX]);
        int minute = Integer.parseInt(split[MINUTE_INDEX]);
        int second = Integer.parseInt(split[SECOND_INDEX]);
        return new Time(new Hour(hour), new Minute(minute), new Second(second));
    }

    public Time next() {
        Second nextSecond = second.next();
        Minute nextMinute = minute.next(nextSecond);
        Hour nextHour = hour.next(nextMinute);
        return new Time(nextHour, nextMinute, nextSecond);
    }

    @Override
    public String toString() {
        return String.format("%02d:%02d:%02d", hour.getHour(), minute.getMinute(), second.getSecond());
    }
}

class Hour {

    private final int hour;

    public Hour(int hour) {
        this.hour = hour;
    }

    public Hour next(Minute minute) {
        int nextHour = hour + add(minute);
        if (nextHour >= 24) {
            return new Hour(0);
        }
        return new Hour(nextHour);
    }

    private int add(Minute minute) {
        return minute.hasCarry() ? 1 : 0;
    }

    public int getHour() {
        return hour;
    }
}

class Minute {

    private final int minute;
    private final boolean carry;

    public Minute(int minute, boolean carry) {
        this.minute = minute;
        this.carry = carry;
    }

    public Minute(int minute) {
        this(minute, false);
    }

    public Minute next(Second second) {
        int nextMinute = minute + add(second);
        if (nextMinute >= 60) {
            return new Minute(0, true);
        }
        return new Minute(nextMinute);
    }

    private int add(Second second) {
        return second.hasCarry() ? 1 : 0;
    }

    public int getMinute() {
        return minute;
    }

    public boolean hasCarry() {
        return carry;
    }
}

class Second {

    private final int second;
    private final boolean carry;

    public Second(int second, boolean carry) {
        this.second = second;
        this.carry = carry;
    }

    public Second(int second) {
        this(second, false);
    }

    public Second next() {
        int nextSecond = second + 1;
        if (nextSecond >= 60) {
            return new Second(0, true);
        }
        return new Second(nextSecond);
    }

    public int getSecond() {
        return second;
    }

    public boolean hasCarry() {
        return carry;
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
    @MethodSource("provideTime")
    @DisplayName("시간, 분, 초를 받으면 1초 후의 시간을 반환한다.")
    void timeReturnsOneSecondAfterTime(String timeString, String expected) {
        Time sut = Time.from(timeString);

        String actual = sut.next().toString();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideTime() {
        return Stream.of(
                Arguments.of("12 12 12", "12:12:13"),
                Arguments.of("12 12 15", "12:12:16"),
                Arguments.of("0 0 0", "00:00:01"),
                Arguments.of("0 0 59", "00:01:00"),
                Arguments.of("23 59 59", "00:00:00")
        );
    }
}

class Time {

    private static final String DELIMITER = " ";
    private static final int HOUR_INDEX = 0;
    private static final int MINUTE_INDEX = 1;
    private static final int SECOND_INDEX = 2;

    private final Hour hour;
    private final Minute minute;
    private final Second second;

    public Time(Hour hour, Minute minute, Second second) {
        this.hour = hour;
        this.minute = minute;
        this.second = second;
    }

    public static Time from(String timeString) {
        String[] split = timeString.split(DELIMITER);
        int hour = Integer.parseInt(split[HOUR_INDEX]);
        int minute = Integer.parseInt(split[MINUTE_INDEX]);
        int second = Integer.parseInt(split[SECOND_INDEX]);
        return new Time(new Hour(hour), new Minute(minute), new Second(second));
    }

    public Time next() {
        Second nextSecond = second.next();
        Minute nextMinute = minute.next(nextSecond);
        Hour nextHour = hour.next(nextMinute);
        return new Time(nextHour, nextMinute, nextSecond);
    }

    @Override
    public String toString() {
        return String.format("%02d:%02d:%02d", hour.getHour(), minute.getMinute(), second.getSecond());
    }
}

class Hour {

    private final int hour;

    public Hour(int hour) {
        this.hour = hour;
    }

    public Hour next(Minute minute) {
        int nextHour = hour + add(minute);
        if (nextHour >= 24) {
            return new Hour(0);
        }
        return new Hour(nextHour);
    }

    private int add(Minute minute) {
        return minute.hasCarry() ? 1 : 0;
    }

    public int getHour() {
        return hour;
    }
}

class Minute {

    private final int minute;
    private final boolean carry;

    public Minute(int minute, boolean carry) {
        this.minute = minute;
        this.carry = carry;
    }

    public Minute(int minute) {
        this(minute, false);
    }

    public Minute next(Second second) {
        int nextMinute = minute + add(second);
        if (nextMinute >= 60) {
            return new Minute(0, true);
        }
        return new Minute(nextMinute);
    }

    private int add(Second second) {
        return second.hasCarry() ? 1 : 0;
    }

    public int getMinute() {
        return minute;
    }

    public boolean hasCarry() {
        return carry;
    }
}

class Second {

    private final int second;
    private final boolean carry;

    public Second(int second, boolean carry) {
        this.second = second;
        this.carry = carry;
    }

    public Second(int second) {
        this(second, false);
    }

    public Second next() {
        int nextSecond = second + 1;
        if (nextSecond >= 60) {
            return new Second(0, true);
        }
        return new Second(nextSecond);
    }

    public int getSecond() {
        return second;
    }

    public boolean hasCarry() {
        return carry;
    }
}
~~~
