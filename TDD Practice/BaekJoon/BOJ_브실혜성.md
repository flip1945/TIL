# 브실혜성 (Bronze 3)

### 문제 설명

혜성처럼 나타난 브실컵의 아이돌 브실이를 보기 위해 전국 각지의 사람들이 천문대로 모였다. 브실이에게 "혜성처럼 나타난" 이라는 수식어가 붙은 이유는 혜성처럼 주기적으로 관측할 수 있기 때문이라고 한다. 오늘 브실이를 볼 수 있다고 할 때, 브실이를 다시 볼 수 있는 날짜를 구하자.

편의를 위해 이 문제에서 한 달은 $30$일, $1$년은 $12$달(즉, $1$년은 $360$일)로 가정하자.

출처 : https://www.acmicpc.net/problem/29722

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        CometDate currentCometDate = CometDate.from(scanner.nextLine());
        int plusDays = Integer.parseInt(scanner.nextLine());

        CometDate nextCometDate = currentCometDate.plusDays(plusDays);
        System.out.print(nextCometDate);
    }
}

class CometDate {

    private static final String DATE_DELIMITER = "-";
    private static final String DATE_STRING_FORMAT = "%04d-%02d-%02d";
    private static final int YEAR_INDEX = 0;
    private static final int MONTH_INDEX = 1;
    private static final int DAY_OF_MONTH_INDEX = 2;
    private static final int NUMBER_OF_DAYS_IN_A_YEAR = 360;
    private static final int NUMBER_OF_DAYS_IN_A_MONTH = 30;

    private final int year;
    private final int month;
    private final int dayOfMonth;

    private CometDate(int year, int month, int dayOfMonth) {
        this.year = year;
        this.month = month;
        this.dayOfMonth = dayOfMonth;
    }

    public static CometDate from(String currentDate) {
        String[] splitDate = currentDate.split(DATE_DELIMITER);
        return new CometDate(Integer.parseInt(splitDate[YEAR_INDEX]), Integer.parseInt(splitDate[MONTH_INDEX]),
                Integer.parseInt(splitDate[DAY_OF_MONTH_INDEX])
        );
    }

    public String toString() {
        return String.format(DATE_STRING_FORMAT, year, month, dayOfMonth);
    }

    public CometDate plusDays(int plusDays) {
        int plusTotalDays = getPlusTotalDays(plusDays);
        return new CometDate(getYearOfTotalDays(plusTotalDays), getMonthOfTotalDays(getPlusTotalDays(plusDays)),
                getDayOfTotalDays(getPlusTotalDays(plusDays)));
    }

    private int getPlusTotalDays(int plusDays) {
        return getTotalDays() + plusDays;
    }

    private int getTotalDays() {
        return (year - 1) * NUMBER_OF_DAYS_IN_A_YEAR + (month - 1) * NUMBER_OF_DAYS_IN_A_MONTH + dayOfMonth - 1;
    }

    private int getYearOfTotalDays(int totalDays) {
        return totalDays / NUMBER_OF_DAYS_IN_A_YEAR + 1;
    }

    private int getMonthOfTotalDays(int totalDays) {
        return totalDays % NUMBER_OF_DAYS_IN_A_YEAR / NUMBER_OF_DAYS_IN_A_MONTH + 1;
    }

    private int getDayOfTotalDays(int totalDays) {
        return totalDays % NUMBER_OF_DAYS_IN_A_MONTH + 1;
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
    @MethodSource("provideCurrentDateAndPlusDays")
    @DisplayName("현재 날짜에 주어진 일수를 더해야 한다.")
    void shouldPlusDate(String currentDate, int plusDays, String expected) {
        CometDate sut = CometDate.from(currentDate);

        CometDate actual = sut.plusDays(plusDays);

        assertEquals(expected, actual.toString());
    }

    private static Stream<Arguments> provideCurrentDateAndPlusDays() {
        return Stream.of(
                Arguments.of("2023-01-01", 1, "2023-01-02"),
                Arguments.of("2023-01-01", 2, "2023-01-03"),
                Arguments.of("2023-01-01", 20, "2023-01-21"),
                Arguments.of("2023-01-01", 30, "2023-02-01"),
                Arguments.of("2023-01-01", 45, "2023-02-16"),
                Arguments.of("2023-01-01", 60, "2023-03-01"),
                Arguments.of("2023-11-01", 60, "2024-01-01"),
                Arguments.of("2023-12-30", 1, "2024-01-01"),
                Arguments.of("2023-07-08", 30, "2023-08-08"),
                Arguments.of("2023-07-08", 360, "2024-07-08"),
                Arguments.of("3000-12-30", 1_000_000, "5778-10-10"),
                Arguments.of("3000-12-30", 1, "3001-01-01"),
                Arguments.of("1001-01-30", 30, "1001-02-30"),
                Arguments.of("1001-01-30", 31, "1001-03-01"),
                Arguments.of("3000-12-30", 90, "3001-03-30"),
                Arguments.of("3000-01-30", 1, "3000-02-01"),
                Arguments.of("3000-01-01", 29, "3000-01-30"),
                Arguments.of("3000-12-28", 1, "3000-12-29"),
                Arguments.of("3000-12-29", 1, "3000-12-30"),
                Arguments.of("2023-12-27", 1, "2023-12-28"),
                Arguments.of("0001-01-01", 1, "0001-01-02")
        );
    }
}

class CometDate {

    private static final String DATE_DELIMITER = "-";
    private static final String DATE_STRING_FORMAT = "%04d-%02d-%02d";
    private static final int YEAR_INDEX = 0;
    private static final int MONTH_INDEX = 1;
    private static final int DAY_OF_MONTH_INDEX = 2;
    private static final int NUMBER_OF_DAYS_IN_A_YEAR = 360;
    private static final int NUMBER_OF_DAYS_IN_A_MONTH = 30;

    private final int year;
    private final int month;
    private final int dayOfMonth;

    private CometDate(int year, int month, int dayOfMonth) {
        this.year = year;
        this.month = month;
        this.dayOfMonth = dayOfMonth;
    }

    public static CometDate from(String currentDate) {
        String[] splitDate = currentDate.split(DATE_DELIMITER);
        return new CometDate(Integer.parseInt(splitDate[YEAR_INDEX]), Integer.parseInt(splitDate[MONTH_INDEX]),
                Integer.parseInt(splitDate[DAY_OF_MONTH_INDEX])
        );
    }

    public String toString() {
        return String.format(DATE_STRING_FORMAT, year, month, dayOfMonth);
    }

    public CometDate plusDays(int plusDays) {
        int plusTotalDays = getPlusTotalDays(plusDays);
        return new CometDate(getYearOfTotalDays(plusTotalDays), getMonthOfTotalDays(getPlusTotalDays(plusDays)),
                getDayOfTotalDays(getPlusTotalDays(plusDays)));
    }

    private int getPlusTotalDays(int plusDays) {
        return getTotalDays() + plusDays;
    }

    private int getTotalDays() {
        return (year - 1) * NUMBER_OF_DAYS_IN_A_YEAR + (month - 1) * NUMBER_OF_DAYS_IN_A_MONTH + dayOfMonth - 1;
    }

    private int getYearOfTotalDays(int totalDays) {
        return totalDays / NUMBER_OF_DAYS_IN_A_YEAR + 1;
    }

    private int getMonthOfTotalDays(int totalDays) {
        return totalDays % NUMBER_OF_DAYS_IN_A_YEAR / NUMBER_OF_DAYS_IN_A_MONTH + 1;
    }

    private int getDayOfTotalDays(int totalDays) {
        return totalDays % NUMBER_OF_DAYS_IN_A_MONTH + 1;
    }
}
~~~
