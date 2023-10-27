# Race Results (Silver 5)

### 문제 설명

The herd has run its first marathon!  The N (1 <= N <= 5,000) times have been posted in the form of Hours (0 <= Hours <= 99), Minutes (0 <= Minutes <= 59), and Seconds (0 <= Seconds <= 59). Bessie must sort them (by Hours, Minutes, and Seconds) into ascending order, smallest times first.

Consider a simple example with times from a smaller herd of just 3 cows (note that cows do not run 26.2 miles so very quickly):

```
11:20:20
11:15:12
14:20:14
```

The proper sorting result is:

```
11:15:12
11:20:20
14:20:14
```

출처 : https://www.acmicpc.net/problem/5939

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> raceResult = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());

        Race race = Race.from(raceResult);
        race.sortedResult()
                .forEach(System.out::println);
    }
}

class Race {

    private final List<Time> times;

    public Race(List<Time> times) {
        this.times = times;
    }

    public static Race from(List<String> raceResults) {
        List<Time> times = raceResults.stream()
                .map(Time::from)
                .collect(Collectors.toList());
        return new Race(times);
    }

    public List<String> sortedResult() {
        return times.stream()
                .sorted()
                .map(Time::toString)
                .collect(Collectors.toList());
    }
}

class Time implements Comparable<Time> {

    private static final String DELIMITER = " ";
    private static final int HOUR_INDEX = 0;
    private static final int MINUTE_INDEX = 1;
    private static final int SECOND_INDEX = 2;

    private final int hour;
    private final int minute;
    private final int second;

    public Time(int hour, int minute, int second) {
        this.hour = hour;
        this.minute = minute;
        this.second = second;
    }

    public static Time from(String time) {
        String[] splitTime = time.split(DELIMITER);
        int hour = Integer.parseInt(splitTime[HOUR_INDEX]);
        int minute = Integer.parseInt(splitTime[MINUTE_INDEX]);
        int second = Integer.parseInt(splitTime[SECOND_INDEX]);
        return new Time(hour, minute, second);
    }

    @Override
    public int compareTo(Time o) {
        int result = Integer.compare(hour, o.hour);
        if (result == 0) {
            result = Integer.compare(minute, o.minute);
            if (result == 0) {
                return Integer.compare(second, o.second);
            }
        }
        return result;
    }

    @Override
    public String toString() {
        return hour + DELIMITER + minute + DELIMITER + second;
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

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideRaceResults")
    @DisplayName("레이스 경기의 결과를 정렬해서 반환한다.")
    void raceReturnSortedRaceResults(List<String> raceResults, List<String> expected) {
        Race sut = Race.from(raceResults);

        List<String> actual = sut.sortedResult();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideRaceResults() {
        return Stream.of(
                Arguments.of(List.of("0 0 1"), List.of("0 0 1")),
                Arguments.of(List.of("10 0 1", "9 0 1"), List.of("9 0 1", "10 0 1")),
                Arguments.of(List.of("1 0 1", "0 0 11"), List.of("0 0 11", "1 0 1")),
                Arguments.of(List.of("11 20 20", "11 15 12", "14 20 14"), List.of("11 15 12", "11 20 20", "14 20 14"))
        );
    }
}

class Race {

    private final List<Time> times;

    public Race(List<Time> times) {
        this.times = times;
    }

    public static Race from(List<String> raceResults) {
        List<Time> times = raceResults.stream()
                .map(Time::from)
                .collect(Collectors.toList());
        return new Race(times);
    }

    public List<String> sortedResult() {
        return times.stream()
                .sorted()
                .map(Time::toString)
                .collect(Collectors.toList());
    }
}

class Time implements Comparable<Time> {

    private static final String DELIMITER = " ";
    private static final int HOUR_INDEX = 0;
    private static final int MINUTE_INDEX = 1;
    private static final int SECOND_INDEX = 2;

    private final int hour;
    private final int minute;
    private final int second;

    public Time(int hour, int minute, int second) {
        this.hour = hour;
        this.minute = minute;
        this.second = second;
    }

    public static Time from(String time) {
        String[] splitTime = time.split(DELIMITER);
        int hour = Integer.parseInt(splitTime[HOUR_INDEX]);
        int minute = Integer.parseInt(splitTime[MINUTE_INDEX]);
        int second = Integer.parseInt(splitTime[SECOND_INDEX]);
        return new Time(hour, minute, second);
    }

    @Override
    public int compareTo(Time o) {
        int result = Integer.compare(hour, o.hour);
        if (result == 0) {
            result = Integer.compare(minute, o.minute);
            if (result == 0) {
                return Integer.compare(second, o.second);
            }
        }
        return result;
    }

    @Override
    public String toString() {
        return hour + DELIMITER + minute + DELIMITER + second;
    }
}
~~~
