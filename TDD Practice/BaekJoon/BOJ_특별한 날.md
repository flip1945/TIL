# 특별한 날 (Bronze 4)

### 문제 설명

2월 18일은 올해 CCC에 있어서 특별한 날이다.

사용자로부터 정수인 월과 일을 입력받아 날짜가 2월 18일인지 전인지 후인지를 출력하는 프로그램이다.

만약 날짜가 2월 18일 전이면, "Before"을 출력한다. 만약 날짜가 2월 18일 후면, "After"을 출력한다. 만약 2월 18일이라면 "Special" 을 출력한다.

출처 : https://www.acmicpc.net/problem/10768

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int month = scanner.nextInt();
        int dayOfMonth = scanner.nextInt();

        SpecialDay specialDay = SpecialDay.of(month, dayOfMonth);
        System.out.println(specialDay.check());
    }
}

class SpecialDay {

    private static final int SPECIAL_MONTH = 2;
    private static final int SPECIAL_DAY_OF_MONTH = 18;
    private static final String SPECIAL_DAY = "Special";
    private static final String BEFORE_DAY = "Before";
    private static final String AFTER_DAY = "After";

    private final int month;
    private final int dayOfMonth;

    private SpecialDay(int month, int dayOfMonth) {
        this.month = month;
        this.dayOfMonth = dayOfMonth;
    }

    public static SpecialDay of(int month, int dayOfMonth) {
        return new SpecialDay(month, dayOfMonth);
    }

    public String check() {
        if (isSpecialDay()) {
            return SPECIAL_DAY;
        }
        return checkBeforeOrAfter();
    }

    private boolean isSpecialDay() {
        return month == SPECIAL_MONTH && dayOfMonth == SPECIAL_DAY_OF_MONTH;
    }

    private String checkBeforeOrAfter() {
        if (month == SPECIAL_MONTH) {
            return checkDayOfMonth();
        }
        return checkMonth();
    }

    private String checkDayOfMonth() {
        return dayOfMonth < SPECIAL_DAY_OF_MONTH ? BEFORE_DAY : AFTER_DAY;
    }

    private String checkMonth() {
        return month < SPECIAL_MONTH ? BEFORE_DAY : AFTER_DAY;
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
    @MethodSource("provideMonthAndDay")
    @DisplayName("날짜가 주어지면 특별한 날인지 전인지 후인지를 판별한다.")
    void shouldCheckWhetherSpecialDayOrBeforeOrAfter(int month, int dayOfMonth, String expected) {
        SpecialDay sut = SpecialDay.of(month, dayOfMonth);

        String actual = sut.check();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideMonthAndDay() {
        return Stream.of(
                Arguments.of(1, 1, "Before"),
                Arguments.of(1, 19, "Before"),
                Arguments.of(3, 1, "After"),
                Arguments.of(2, 1, "Before"),
                Arguments.of(12, 12, "After"),
                Arguments.of(12, 31, "After"),
                Arguments.of(2, 18, "Special"),
                Arguments.of(2, 17, "Before"),
                Arguments.of(2, 19, "After")
        );
    }
}

class SpecialDay {

    private static final int SPECIAL_MONTH = 2;
    private static final int SPECIAL_DAY_OF_MONTH = 18;
    private static final String SPECIAL_DAY = "Special";
    private static final String BEFORE_DAY = "Before";
    private static final String AFTER_DAY = "After";

    private final int month;
    private final int dayOfMonth;

    private SpecialDay(int month, int dayOfMonth) {
        this.month = month;
        this.dayOfMonth = dayOfMonth;
    }

    public static SpecialDay of(int month, int dayOfMonth) {
        return new SpecialDay(month, dayOfMonth);
    }

    public String check() {
        if (isSpecialDay()) {
            return SPECIAL_DAY;
        }
        return checkBeforeOrAfter();
    }

    private boolean isSpecialDay() {
        return month == SPECIAL_MONTH && dayOfMonth == SPECIAL_DAY_OF_MONTH;
    }

    private String checkBeforeOrAfter() {
        if (month == SPECIAL_MONTH) {
            return checkDayOfMonth();
        }
        return checkMonth();
    }

    private String checkDayOfMonth() {
        return dayOfMonth < SPECIAL_DAY_OF_MONTH ? BEFORE_DAY : AFTER_DAY;
    }

    private String checkMonth() {
        return month < SPECIAL_MONTH ? BEFORE_DAY : AFTER_DAY;
    }
}
~~~
