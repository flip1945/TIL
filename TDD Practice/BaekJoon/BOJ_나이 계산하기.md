# 나이 계산하기 (Bronze 4)

### 문제 설명

한국에서 나이는 총 3가지 종류가 있다.

* 만 나이: 국제적인 표준 방법이다. 한국에서도 법에서는 만 나이만을 사용한다.
* 세는 나이: 한국에서 보통 나이를 물어보면 세는 나이를 의미한다.
* 연 나이: 법률에서 일괄적으로 사람을 구분하기 위해서 사용하는 나이이다.

만 나이는 생일을 기준으로 계산한다. 어떤 사람이 태어났을 때, 그 사람의 나이는 0세이고, 생일이 지날 때마다 1세가 증가한다. 예를 들어, 생일이 2003년 3월 5일인 사람은 2004년 3월 4일까지 0세이고, 2004년 3월 5일부터 2005년 3월 4일까지 1세이다.

세는 나이는 생년을 기준으로 계산한다. 어떤 사람이 태어났을 때, 그 사람의 나이는 1세이고, 연도가 바뀔 때마다 1세가 증가한다. 예를 들어, 생일이 2003년 3월 5일인 사람은 2003년 12월 31일까지 1세이고, 2004년 1월 1일부터 2004년 12월 31일까지 2세이다.

연 나이는 생년을 기준으로 계산하고, 현재 연도에서 생년을 뺀 값이다. 예를 들어, 생일이 2003년 3월 5일인 사람은 2003년 12월 31일까지 0세이고, 2004년 1월 1일부터 2004년 12월 31일까지 1세이다.

어떤 사람의 생년월일과 기준 날짜가 주어졌을 때, 기준 날짜를 기준으로 그 사람의 만 나이, 세는 나이, 연 나이를 모두 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/16199

---

#### 풀이
~~~java
import java.time.LocalDate;
import java.time.Period;
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String birthDay = scanner.nextLine();
        String currentDay = scanner.nextLine();
        Age age = Age.of(birthDay, currentDay);

        System.out.println(age.getAges());
    }
}

class Age {

    private static final String DELIMITER = "\n";
    private static final int KOREAN_AGE_OFFSET = 1;

    private final Day birthDay;
    private final Day currentDay;

    public Age(Day birthDay, Day currentDay) {
        this.birthDay = birthDay;
        this.currentDay = currentDay;
    }

    public static Age of(String birthday, String currentDay) {
        return new Age(Day.from(birthday), Day.from(currentDay));
    }

    public String getAges() {
        return getAge() + DELIMITER + getKoreanAge() + DELIMITER + getYearAge();
    }

    private int getAge() {
        LocalDate birthDate = LocalDate.of(birthDay.getYear(), birthDay.getMonth(), birthDay.getDayOfMonth());
        LocalDate currentDate = LocalDate.of(currentDay.getYear(), currentDay.getMonth(), currentDay.getDayOfMonth());
        return Period.between(birthDate, currentDate).getYears();
    }

    private int getKoreanAge() {
        return getYearAge() + KOREAN_AGE_OFFSET;
    }

    private int getYearAge() {
        return currentDay.getYear() - birthDay.getYear();
    }
}

class Day {

    private static final String DELIMITER = " ";
    private static final int YEAR_INDEX = 0;
    private static final int MONTH_INDEX = 1;
    private static final int DAY_OF_MONTH_INDEX = 2;

    private final int year;
    private final int month;
    private final int dayOfMonth;

    public Day(int year, int month, int dayOfMonth) {
        this.year = year;
        this.month = month;
        this.dayOfMonth = dayOfMonth;
    }

    public static Day from(String day) {
        String[] splitDays = day.split(DELIMITER);
        return new Day(Integer.parseInt(splitDays[YEAR_INDEX]), Integer.parseInt(splitDays[MONTH_INDEX]),
                Integer.parseInt(splitDays[DAY_OF_MONTH_INDEX]));
    }

    public int getYear() {
        return year;
    }

    public int getMonth() {
        return month;
    }

    public int getDayOfMonth() {
        return dayOfMonth;
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

import java.time.LocalDate;
import java.time.Period;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideBirthDay")
    @DisplayName("생일과 현재 날짜가 주어지면 만 나이, 세는 나이, 연 나이를 반환한다.")
    void ageCalculatesAgeGivenBirthdayAndCurrentDay(String birthday, String currentDay, String expected) {
        Age sut = Age.of(birthday, currentDay);

        String actual = sut.getAges();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideBirthDay() {
        return Stream.of(
                Arguments.of("2000 1 1", "2000 1 2", "0\n1\n0"),
                Arguments.of("2000 1 1", "2001 1 1", "1\n2\n1"),
                Arguments.of("2003 3 5", "2003 4 5", "0\n1\n0"),
                Arguments.of("2003 3 5", "2004 1 1", "0\n2\n1"),
                Arguments.of("2005 1 1", "2007 1 1", "2\n3\n2"),
                Arguments.of("2005 12 31", "2007 1 2", "1\n3\n2"),
                Arguments.of("2005 12 31", "2007 1 1", "1\n3\n2")
        );
    }
}

class Age {

    private static final String DELIMITER = "\n";
    private static final int KOREAN_AGE_OFFSET = 1;

    private final Day birthDay;
    private final Day currentDay;

    public Age(Day birthDay, Day currentDay) {
        this.birthDay = birthDay;
        this.currentDay = currentDay;
    }

    public static Age of(String birthday, String currentDay) {
        return new Age(Day.from(birthday), Day.from(currentDay));
    }

    public String getAges() {
        return getAge() + DELIMITER + getKoreanAge() + DELIMITER + getYearAge();
    }

    private int getAge() {
        LocalDate birthDate = LocalDate.of(birthDay.getYear(), birthDay.getMonth(), birthDay.getDayOfMonth());
        LocalDate currentDate = LocalDate.of(currentDay.getYear(), currentDay.getMonth(), currentDay.getDayOfMonth());
        return Period.between(birthDate, currentDate).getYears();
    }

    private int getKoreanAge() {
        return getYearAge() + KOREAN_AGE_OFFSET;
    }

    private int getYearAge() {
        return currentDay.getYear() - birthDay.getYear();
    }
}

class Day {

    private static final String DELIMITER = " ";
    private static final int YEAR_INDEX = 0;
    private static final int MONTH_INDEX = 1;
    private static final int DAY_OF_MONTH_INDEX = 2;

    private final int year;
    private final int month;
    private final int dayOfMonth;

    public Day(int year, int month, int dayOfMonth) {
        this.year = year;
        this.month = month;
        this.dayOfMonth = dayOfMonth;
    }

    public static Day from(String day) {
        String[] splitDays = day.split(DELIMITER);
        return new Day(Integer.parseInt(splitDays[YEAR_INDEX]), Integer.parseInt(splitDays[MONTH_INDEX]),
                Integer.parseInt(splitDays[DAY_OF_MONTH_INDEX]));
    }

    public int getYear() {
        return year;
    }

    public int getMonth() {
        return month;
    }

    public int getDayOfMonth() {
        return dayOfMonth;
    }
}
~~~
