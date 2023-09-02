# 타임 카드 (Bronze 4)

### 문제 설명

JOI 상사는 직원의 근무시간을 타임 카드로 관리하고있다. 직원들은 전용 장비를 사용하여 타임 카드에 출근 시간을 기록한다. 근무를 마치고 퇴근할 때도 타임 카드에 퇴근 시간을 기록한다. 타임카드에서 사용하는 시간단위는 24 시간제를 사용한다.

보안상의 이유로 직원들의 출근 시간은 7시 이후이다. 또한, 모든 직원은 23시 이전에 퇴근한다. 직원의 퇴근 시간은 항상 출근 시간보다 늦다.

입력으로 JOI 상사의 3 명의 직원 A 씨, B 씨, C 씨의 출근 시간과 퇴근 시간이 주어 졌을 때 각 직원의 근무시간을 계산하는 프로그램을 작성하라.

출처 : https://www.acmicpc.net/problem/5575

---

#### 풀이
~~~java
import java.time.*;
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        for (int i = 0; i < 3; i++) {
            TimeCard timeCard = TimeCard.of(scanner.nextInt(), scanner.nextInt(), scanner.nextInt(), scanner.nextInt(),
                    scanner.nextInt(), scanner.nextInt());

            System.out.println(timeCard.calculateWorkingTime());
        }
    }
}

class TimeCard {

    private final LocalTime startTime;
    private final LocalTime endTime;

    private TimeCard(LocalTime startTime, LocalTime endTime) {
        this.startTime = startTime;
        this.endTime = endTime;
    }

    public static TimeCard of(int startHour, int startMinute, int startSecond,
                              int endHour, int endMinute, int endSecond) {
        LocalTime startTIme = LocalTime.of(startHour, startMinute, startSecond);
        LocalTime endTime = LocalTime.of(endHour, endMinute, endSecond);
        return new TimeCard(startTIme, endTime);
    }

    public String calculateWorkingTime() {
        return WorkingTime.of(startTime, endTime).toString();
    }
}

class WorkingTime {

    private static final int SECONDS_OF_HOURS = 3600;
    private static final int SECONDS_OF_MINUTES = 60;
    private static final String WORKING_TIME_FORMAT = "%d %d %d";

    private final Duration workingTime;

    private WorkingTime(Duration workingTime) {
        this.workingTime = workingTime;
    }

    public static WorkingTime of(LocalTime startTime, LocalTime endTime) {
        return new WorkingTime(Duration.between(startTime, endTime));
    }

    @Override
    public String toString() {
        return String.format(WORKING_TIME_FORMAT, getWorkingHours(), getWorkingMinutes(), getWorkingSeconds());
    }

    private long getWorkingHours() {
        return workingTime.getSeconds() / SECONDS_OF_HOURS;
    }

    private long getWorkingMinutes() {
        return (workingTime.getSeconds() % SECONDS_OF_HOURS) / SECONDS_OF_MINUTES;
    }

    private long getWorkingSeconds() {
        return workingTime.getSeconds() % SECONDS_OF_MINUTES;
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

import java.time.*;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideStartTimeAndEndTime")
    @DisplayName("직원의 출퇴근 시간이 주어졌을 때 근무시간을 계산한다.")
    void shouldCalculateWorkingTime(int startHour, int startMinute, int startSecond, int endHour, int endMinute,
                                    int endSecond, String expected) {
        TimeCard sut = TimeCard.of(startHour, startMinute, startSecond, endHour, endMinute, endSecond);

        String actual = sut.calculateWorkingTime();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideStartTimeAndEndTime() {
        return Stream.of(
                Arguments.of(0, 0, 0, 1, 0, 0, "1 0 0"),
                Arguments.of(0, 0, 0, 2, 0, 0, "2 0 0"),
                Arguments.of(9, 0, 0, 18, 0, 0, "9 0 0"),
                Arguments.of(9, 0, 1, 18, 0, 0, "8 59 59"),
                Arguments.of(12, 14, 52, 12, 15, 30, "0 0 38"),
                Arguments.of(0, 0, 0, 0, 30, 0, "0 30 0"),
                Arguments.of(0, 0, 1, 0, 30, 0, "0 29 59"),
                Arguments.of(0, 0, 0, 0, 0, 1, "0 0 1"),
                Arguments.of(0, 0, 0, 0, 0, 0, "0 0 0")
        );
    }
}

class TimeCard {

    private final LocalTime startTime;
    private final LocalTime endTime;

    private TimeCard(LocalTime startTime, LocalTime endTime) {
        this.startTime = startTime;
        this.endTime = endTime;
    }

    public static TimeCard of(int startHour, int startMinute, int startSecond,
                              int endHour, int endMinute, int endSecond) {
        LocalTime startTIme = LocalTime.of(startHour, startMinute, startSecond);
        LocalTime endTime = LocalTime.of(endHour, endMinute, endSecond);
        return new TimeCard(startTIme, endTime);
    }

    public String calculateWorkingTime() {
        return WorkingTime.of(startTime, endTime).toString();
    }
}

class WorkingTime {

    private static final int SECONDS_OF_HOURS = 3600;
    private static final int SECONDS_OF_MINUTES = 60;
    private static final String WORKING_TIME_FORMAT = "%d %d %d";

    private final Duration workingTime;

    private WorkingTime(Duration workingTime) {
        this.workingTime = workingTime;
    }

    public static WorkingTime of(LocalTime startTime, LocalTime endTime) {
        return new WorkingTime(Duration.between(startTime, endTime));
    }

    @Override
    public String toString() {
        return String.format(WORKING_TIME_FORMAT, getWorkingHours(), getWorkingMinutes(), getWorkingSeconds());
    }

    private long getWorkingHours() {
        return workingTime.getSeconds() / SECONDS_OF_HOURS;
    }

    private long getWorkingMinutes() {
        return (workingTime.getSeconds() % SECONDS_OF_HOURS) / SECONDS_OF_MINUTES;
    }

    private long getWorkingSeconds() {
        return workingTime.getSeconds() % SECONDS_OF_MINUTES;
    }
}
~~~
