# 트럭 주차 (Bronze 2)

### 문제 설명

상근이는 트럭을 총 세 대 가지고 있다. 오늘은 트럭을 주차하는데 비용이 얼마나 필요한지 알아보려고 한다.

상근이가 이용하는 주차장은 주차하는 트럭의 수에 따라서 주차 요금을 할인해 준다.

트럭을 한 대 주차할 때는 1분에 한 대당 A원을 내야 한다. 두 대를 주차할 때는 1분에 한 대당 B원, 세 대를 주차할 때는 1분에 한 대당 C원을 내야 한다.

A, B, C가 주어지고, 상근이의 트럭이 주차장에 주차된 시간이 주어졌을 때, 주차 요금으로 얼마를 내야 하는지 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/2979

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int a = scanner.nextInt();
        int b = scanner.nextInt();
        int c = scanner.nextInt();
        List<TimeRange> timeRanges = IntStream.range(0, 3)
                .mapToObj(i -> new TimeRange(scanner.nextInt(), scanner.nextInt()))
                .collect(Collectors.toList());

        System.out.println(calculateParkingFee(a, b, c, timeRanges));
    }

    static int calculateParkingFee(int a, int b, int c, List<TimeRange> timeRanges) {
        return IntStream.range(1, 100)
                .map(time -> calculateWithTime(a, b, c, getWithInCount(timeRanges, time)))
                .sum();
    }

    static int getWithInCount(List<TimeRange> timeRanges, int time) {
        return (int) timeRanges.stream()
                .filter(timeRange -> timeRange.withIn(time))
                .count();
    }

    static int calculateWithTime(int a, int b, int c, int count) {
        switch (count) {
            case 1: return count * a;
            case 2: return count * b;
            case 3: return count * c;
            default: return 0;
        }
    }
}

class TimeRange {
    int startTime;
    int endTime;

    public TimeRange(int startTime, int endTime) {
        this.startTime = startTime;
        this.endTime = endTime;
    }

    public boolean withIn(int time) {
        return time >= startTime && time < endTime;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.List;
import java.util.stream.IntStream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void calculateParkingFeeTest() {
        assertEquals(3, calculateParkingFee(3, 2, 1, List.of(new TimeRange(1, 2))));
        assertEquals(6, calculateParkingFee(3, 2, 1, List.of(new TimeRange(1, 3))));
        assertEquals(7, calculateParkingFee(3, 2, 1, List.of(new TimeRange(1, 2), new TimeRange(1, 3))));
        assertEquals(10, calculateParkingFee(3, 2, 1, List.of(new TimeRange(1, 2), new TimeRange(1, 3), new TimeRange(1, 4))));
        assertEquals(20, calculateParkingFee(6, 4, 2, List.of(new TimeRange(1, 2), new TimeRange(1, 3), new TimeRange(1, 4))));
        assertEquals(6, calculateParkingFee(1, 1, 1, List.of(new TimeRange(1, 2), new TimeRange(1, 3), new TimeRange(1, 4))));
        assertEquals(33, calculateParkingFee(5, 3, 1, List.of(new TimeRange(1, 6), new TimeRange(3, 5), new TimeRange(2, 8))));
        assertEquals(480, calculateParkingFee(10, 8, 6, List.of(new TimeRange(15, 30), new TimeRange(25, 50), new TimeRange(70, 80))));
    }

    private int calculateParkingFee(int a, int b, int c, List<TimeRange> timeRanges) {
        return IntStream.range(1, 100)
                .map(time -> calculateWithTime(a, b, c, getWithInCount(timeRanges, time)))
                .sum();
    }

    private int getWithInCount(List<TimeRange> timeRanges, int time) {
        return (int) timeRanges.stream()
                .filter(timeRange -> timeRange.withIn(time))
                .count();
    }

    private int calculateWithTime(int a, int b, int c, int count) {
        switch (count) {
            case 1: return count * a;
            case 2: return count * b;
            case 3: return count * c;
            default: return 0;
        }
    }
}

class TimeRange {
    int startTime;
    int endTime;

    public TimeRange(int startTime, int endTime) {
        this.startTime = startTime;
        this.endTime = endTime;
    }

    public boolean withIn(int time) {
        return time >= startTime && time < endTime;
    }
}
~~~
