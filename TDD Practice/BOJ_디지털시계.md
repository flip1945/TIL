# 양 한마리... 양 두마리... (Bronze 2)

### 문제 설명

디지털시계는 일반적으로 시각을 “hh:mm:ss”의 형태로 표현한다. hh는 00 이상 23 이하의 값을, mm과 ss는 00 이상 59 이하의 값을 가질 수 있다. 이러한 형태의 시각에서 콜론(“:”)을 제거하면 “hhmmss”라는 정수를 얻을 수 있는데, 이러한 정수를 시계 정수라고 한다. 예를 들어, 오후 5시 5분 13초, 즉 17:05:13의 시계 정수는 170513이고, 오전 0시 7분 37초, 즉 00:07:37의 시계 정수는 737이다.

이 문제에서 시간이란 시각의 구간을 나타낸다. 예를 들어 [00:59:58, 01:01:24]와 같이 시작하는 시각과 끝나는 시각으로 이루어진 구간을 우리는 시간이라고 부른다. (이러한 구간에는 시작하는 시간과 끝나는 시간도 포함된다)

이렇게 시간이 구간으로 주어지면, 그 구간에 포함되는 시계 정수들을 나열할 수 있다. 예를 들어 [00:59:58, 01:01:24]에 포함되는 시계 정수는 5958, 5959, 다음으로 10000 이상 10059 이하, 마지막으로 10100 이상 10124 이하로 총 87개가 된다. 우리는 이처럼 특정한 시간에 포함되는 시계 정수들 중, 3의 배수인 것이 몇 개나 있는지를 알고 싶다.

시간은 자정을 포함할 수도 있다. 즉 [23:59:03, 00:01:24]처럼 시작하는 시각의 시계 정수(235903)가 끝나는 시각의 시계 정수(124)보다 클 수도 있다. 물론 이 경우 이 구간에 포함되는 시계 정수는 235903 이상 235959 이하, 0 이상 59 이하, 100 이상 124 이하가 된다. 모든 구간이 포함하는 시간은 만 하루, 즉 24시간보다는 항상 작다고 가정해도 좋다.

시간이 시작하는 시각과 끝나는 시각으로 주어졌을 때, 이 구간에 포함되는 시계 정수들 중, 3의 배수인 것의 개수를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1942

---

#### 풀이
~~~java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        for (int i = 0; i < 3; i++) {
            StringTokenizer st = new StringTokenizer(bf.readLine());
            String start = st.nextToken();
            String end = st.nextToken();

            System.out.println(getAnswer(start, end));
        }
    }

    private static int getAnswer(String start, String end) {
        Time startTime = new Time(start);
        int endTime = new Time(end).getTimeInt();

        int answer = 0;
        while (startTime.getTimeInt() != endTime) {
            if (isThreeTimes(startTime)) {
                answer++;
            }
            startTime.increaseSecond();
        }
        return isThreeTimes(startTime) ? answer + 1 : answer;
    }

    private static boolean isThreeTimes(Time startTime) {
        return startTime.getTimeInt() % 3 == 0;
    }
}

class Time {
    private final int[] timeInts;
    public Time(String timeString) {
        timeInts = Arrays.stream(timeString.split(":")).mapToInt(Integer::parseInt).toArray();
    }

    public int getTimeInt() {
        int hour = timeInts[0];
        int minute = timeInts[1];
        int second = timeInts[2];
        return hour * 10000 + minute * 100 + second;
    }

    public void increaseSecond() {
        int second = timeInts[2] + 1;
        if (second > 59) {
            second = 0;
            increaseMinute();
        }
        timeInts[2] = second;
    }

    private void increaseMinute() {
        int minute = timeInts[1] + 1;
        if (minute > 59) {
            minute = 0;
            increaseHour();
        }
        timeInts[1] = minute;
    }

    private void increaseHour() {
        int hour = timeInts[0] + 1;
        if (hour > 23) {
            hour = 0;
        }
        timeInts[0] = hour;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Arrays;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void timeTest() {
        String time1 = "00:59:58";
        String time2 = "01:01:24";
        String time3 = "22:47:03";
        String time4 = "01:03:24";
        String time5 = "00:00:09";
        String time6 = "00:03:37";

        assertEquals(5958, new Time(time1).getTimeInt());
        assertEquals(10124, new Time(time2).getTimeInt());
        assertEquals(224703, new Time(time3).getTimeInt());
        assertEquals(10324, new Time(time4).getTimeInt());
        assertEquals(9, new Time(time5).getTimeInt());
        assertEquals(337, new Time(time6).getTimeInt());
    }

    @Test
    void nextTimeTest() {
        Time time1 = new Time("00:59:58");
        Time time2 = new Time("01:01:24");
        Time time3 = new Time("23:59:59");
        Time time4 = new Time("22:59:59");

        time1.increaseSecond();
        time2.increaseSecond();
        time3.increaseSecond();
        time4.increaseSecond();

        assertEquals(5959, time1.getTimeInt());
        assertEquals(10125, time2.getTimeInt());
        assertEquals(0, time3.getTimeInt());
        assertEquals(230000, time4.getTimeInt());
    }

    @Test
    void integrationTest() {
        String startTime1 = "00:59:58";
        String endTime1 = "01:01:24";
        String startTime2 = "22:47:03";
        String endTime2 = "01:03:24";
        String startTime3 = "00:00:09";
        String endTime3 = "00:03:37";
        String startTime4 = "00:00:01";
        String endTime4 = "00:00:03";
        String startTime5 = "00:00:00";
        String endTime5 = "23:59:59";

        assertEquals(29, getAnswer(startTime1, endTime1));
        assertEquals(2727, getAnswer(startTime2, endTime2));
        assertEquals(70, getAnswer(startTime3, endTime3));
        assertEquals(1, getAnswer(startTime4, endTime4));
        assertEquals(28800, getAnswer(startTime5, endTime5));
    }

    private int getAnswer(String start, String end) {
        Time startTime = new Time(start);
        Time endTime = new Time(end);

        int answer = 0;
        while (startTime.getTimeInt() != endTime.getTimeInt()) {
            if (isThreeTimes(startTime)) {
                answer++;
            }
            startTime.increaseSecond();
        }
        return isThreeTimes(startTime) ? answer + 1 : answer;
    }

    private boolean isThreeTimes(Time startTime) {
        return startTime.getTimeInt() % 3 == 0;
    }
}

class Time {
    private final int[] timeInts;
    public Time(String timeString) {
        timeInts = Arrays.stream(timeString.split(":")).mapToInt(Integer::parseInt).toArray();
    }

    public int getTimeInt() {
        int hour = timeInts[0];
        int minute = timeInts[1];
        int second = timeInts[2];
        return hour * 10000 + minute * 100 + second;
    }

    public void increaseSecond() {
        int second = timeInts[2] + 1;
        if (second > 59) {
            second = 0;
            increaseMinute();
        }
        timeInts[2] = second;
    }

    private void increaseMinute() {
        int minute = timeInts[1] + 1;
        if (minute > 59) {
            minute = 0;
            increaseHour();
        }
        timeInts[1] = minute;
    }

    private void increaseHour() {
        int hour = timeInts[0] + 1;
        if (hour > 23) {
            hour = 0;
        }
        timeInts[0] = hour;
    }
}
~~~
