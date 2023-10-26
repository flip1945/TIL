# Birthday Statistics (Bronze 2)

### 문제 설명

In a company, the birthdays of all employees are collected. Your task is to write a program to summarize the number of people that were born in each month.

출처 : https://www.acmicpc.net/problem/9777

---

#### 풀이
~~~java
import java.time.LocalDate;
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        BirthdayStatistics birthdayStatistics = new BirthdayStatistics(inputEmployees(scanner, n));
        System.out.println(birthdayStatistics);
    }

    private static List<String> inputEmployees(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class BirthdayStatistics {

    private static final int MONTH = 12;
    private static final int MONTH_OFFSET = 1;
    private static final String DELIMITER = "\n";

    private final List<String> employees;

    public BirthdayStatistics(List<String> employees) {
        this.employees = employees;
    }

    public int[] statistics() {
        int[] statistics = new int[MONTH];
        for (String employeeInfo : employees) {
            Employee employee = Employee.from(employeeInfo);
            statistics[employee.getBirthMonth() - MONTH_OFFSET]++;
        }
        return statistics;
    }

    @Override
    public String toString() {
        int[] statistics = statistics();
        return IntStream.range(0, MONTH)
                .mapToObj(month -> month + MONTH_OFFSET + " " + statistics[month])
                .collect(Collectors.joining(DELIMITER));
    }
}

class Employee {

    private static final String DELIMITER = " ";
    private static final int ID_INDEX = 0;
    private static final int BIRTHDAY_INDEX = 1;

    private final String id;
    private final Birthday birthday;

    public Employee(String id, Birthday birthday) {
        this.id = id;
        this.birthday = birthday;
    }

    public static Employee from(String employee) {
        String[] split = employee.split(DELIMITER);
        return new Employee(split[ID_INDEX], Birthday.from(split[BIRTHDAY_INDEX]));
    }

    public int getBirthMonth() {
        return birthday.getBirthMonth();
    }
}

class Birthday {

    private static final String DELIMITER = "/";
    private static final int DAY_INDEX = 0;
    private static final int MONTH_INDEX = 1;
    private static final int YEAR_INDEX = 2;

    private final LocalDate birthday;

    public Birthday(LocalDate birthday) {
        this.birthday = birthday;
    }

    public static Birthday from(String birthday) {
        String[] split = birthday.split(DELIMITER);
        int day = Integer.parseInt(split[DAY_INDEX]);
        int month = Integer.parseInt(split[MONTH_INDEX]);
        int year = Integer.parseInt(split[YEAR_INDEX]);
        return new Birthday(LocalDate.of(year, month, day));
    }

    public int getBirthMonth() {
        return birthday.getMonthValue();
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
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideBirthdays")
    @DisplayName("생일 통계는 통계를 반환한다.")
    void birthdayStatisticsReturnsStatistics(List<String> employees, int[] expected) {
        BirthdayStatistics sut = new BirthdayStatistics(employees);

        int[] actual = sut.statistics();

        assertArrayEquals(expected, actual);
    }

    static Stream<Arguments> provideBirthdays() {
        return Stream.of(
                Arguments.of(List.of("1000 1/2/2000"), new int[]{0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}),
                Arguments.of(List.of("1000 1/2/2000", "1001 2/2/2002"), new int[]{0, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}),
                Arguments.of(List.of("1000 1/2/1967", "1012 13/10/1940", "1103 5/1/1965", "2012 16/7/1980",
                        "1125 18/9/1979", "1235 10/10/1976", "1400 16/11/1973", "1013 5/1/1965", "2109 28/7/1958",
                        "2155 10/12/1970"), new int[]{2, 1, 0, 0, 0, 0, 2, 0, 1, 2, 1, 1})
        );
    }
}

class BirthdayStatistics {

    private static final int MONTH = 12;
    private static final int MONTH_OFFSET = 1;
    public static final String DELIMITER = "\n";

    private final List<String> employees;

    public BirthdayStatistics(List<String> employees) {
        this.employees = employees;
    }

    public int[] statistics() {
        int[] statistics = new int[MONTH];
        for (String employeeInfo : employees) {
            Employee employee = Employee.from(employeeInfo);
            statistics[employee.getBirthMonth() - MONTH_OFFSET]++;
        }
        return statistics;
    }

    @Override
    public String toString() {
        int[] statistics = statistics();
        return IntStream.range(0, MONTH)
                .mapToObj(month -> month + MONTH_OFFSET + " " + statistics[month])
                .collect(Collectors.joining(DELIMITER));
    }
}

class Employee {

    private static final String DELIMITER = " ";
    private static final int ID_INDEX = 0;
    private static final int BIRTHDAY_INDEX = 1;

    private final String id;
    private final Birthday birthday;

    public Employee(String id, Birthday birthday) {
        this.id = id;
        this.birthday = birthday;
    }

    public static Employee from(String employee) {
        String[] split = employee.split(DELIMITER);
        return new Employee(split[ID_INDEX], Birthday.from(split[BIRTHDAY_INDEX]));
    }

    public int getBirthMonth() {
        return birthday.getBirthMonth();
    }
}

class Birthday {

    private static final String DELIMITER = "/";
    private static final int DAY_INDEX = 0;
    private static final int MONTH_INDEX = 1;
    private static final int YEAR_INDEX = 2;

    private final LocalDate birthday;

    public Birthday(LocalDate birthday) {
        this.birthday = birthday;
    }

    public static Birthday from(String birthday) {
        String[] split = birthday.split(DELIMITER);
        int day = Integer.parseInt(split[DAY_INDEX]);
        int month = Integer.parseInt(split[MONTH_INDEX]);
        int year = Integer.parseInt(split[YEAR_INDEX]);
        return new Birthday(LocalDate.of(year, month, day));
    }

    public int getBirthMonth() {
        return birthday.getMonthValue();
    }
}
~~~
