# 생일 (Silver 5)

### 문제 설명

어떤 반에 있는 학생들의 생일이 주어졌을 때, 가장 나이가 적은 사람과 가장 많은 사람을 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/5635

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> studentInfos = IntStream.range(0, n)
            .mapToObj(i -> scanner.nextLine())
            .collect(Collectors.toList());

        BirthdayCalculator calculator = new BirthdayCalculator(studentInfos);
        System.out.println(calculator.getYoungestStudentName());
        System.out.println(calculator.getOldestStudentName());
    }
}

class BirthdayCalculator {
    private final List<Student> sortedStudentByBirthday;

    public BirthdayCalculator(List<String> studentInfos) {
        this.sortedStudentByBirthday = getSortedStudentByBirthday(studentInfos);
    }

    private static List<Student> getSortedStudentByBirthday(List<String> studentInfos) {
        return studentInfos.stream()
            .map(Student::new)
            .sorted()
            .collect(Collectors.toList());
    }

    public String getOldestStudentName() {
        return sortedStudentByBirthday.get(0)
            .getName();
    }

    public String getYoungestStudentName() {
        return sortedStudentByBirthday.get(sortedStudentByBirthday.size() - 1)
            .getName();
    }
}

class Student implements Comparable<Student> {
    private final String name;
    private final Birthday birthday;

    public Student(String studentInfo) {
        String[] splitInfo = studentInfo.split(" ");
        this.name = splitInfo[0];
        this.birthday = new Birthday(Integer.parseInt(splitInfo[1]), Integer.parseInt(splitInfo[2]), Integer.parseInt(splitInfo[3]));
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(Student o) {
        return birthday.compareTo(o.birthday);
    }
}

class Birthday implements Comparable<Birthday> {
    private final int dayOfMonth;
    private final int month;
    private final int year;

    public Birthday(int dayOfMonth, int month, int year) {
        this.dayOfMonth = dayOfMonth;
        this.month = month;
        this.year = year;
    }

    @Override
    public int compareTo(Birthday o) {
        int result = Integer.compare(this.year, o.year);
        if (result != 0) {
            return result;
        }
        result = Integer.compare(this.month, o.month);
        if (result != 0) {
            return result;
        }
        return Integer.compare(this.dayOfMonth, o.dayOfMonth);
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

import static org.junit.jupiter.api.Assertions.*;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class MainTest {

    @ParameterizedTest
    @MethodSource("provideStudentInfosAndOldestStudentName")
    @DisplayName("가장 나이가 많은 학생의 이름을 반환한다.")
    void it_returns_oldest_student_name(List<String> studentInfos, String expected) {
        BirthdayCalculator sut = new BirthdayCalculator(studentInfos);
        
        String actual = sut.getOldestStudentName();
        
        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideStudentInfosAndOldestStudentName() {
        return Stream.of(
            Arguments.of(List.of("a 1 1 2010", "b 1 1 2019", "c 24 3 2023"), "a"),
            Arguments.of(List.of("a 1 1 2010", "b 1 1 2009", "c 24 3 2023"), "b"),
            Arguments.of(List.of("Mickey 1 10 1991", "Alice 30 12 1990", "Tom 15 8 1993", "Jerry 18 9 1990", "Garfield 20 9 1990"), "Jerry")
        );
    }

    @ParameterizedTest
    @MethodSource("provideStudentInfosAndYoungestStudentName")
    @DisplayName("가장 나이가 적은 학생의 이름을 반환한다.")
    void it_returns_youngest_student_name(List<String> studentInfos, String expected) {
        BirthdayCalculator sut = new BirthdayCalculator(studentInfos);

        String actual = sut.getYoungestStudentName();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideStudentInfosAndYoungestStudentName() {
        return Stream.of(
            Arguments.of(List.of("a 1 1 2010", "b 1 1 2019", "c 24 3 2023"), "c"),
            Arguments.of(List.of("a 1 1 2010", "b 1 1 2009", "c 24 3 2023"), "c"),
            Arguments.of(List.of("Mickey 1 10 1991", "Alice 30 12 1990", "Tom 15 8 1993", "Jerry 18 9 1990", "Garfield 20 9 1990"), "Tom")
        );
    }
}

class BirthdayCalculator {
    private final List<Student> sortedStudentByBirthday;

    public BirthdayCalculator(List<String> studentInfos) {
        this.sortedStudentByBirthday = getSortedStudentByBirthday(studentInfos);
    }

    private static List<Student> getSortedStudentByBirthday(List<String> studentInfos) {
        return studentInfos.stream()
            .map(Student::new)
            .sorted()
            .collect(Collectors.toList());
    }

    public String getOldestStudentName() {
        return sortedStudentByBirthday.get(0)
            .getName();
    }

    public String getYoungestStudentName() {
        return sortedStudentByBirthday.get(sortedStudentByBirthday.size() - 1)
            .getName();
    }
}

class Student implements Comparable<Student> {
    private final String name;
    private final Birthday birthday;

    public Student(String studentInfo) {
        String[] splitInfo = studentInfo.split(" ");
        this.name = splitInfo[0];
        this.birthday = new Birthday(Integer.parseInt(splitInfo[1]), Integer.parseInt(splitInfo[2]), Integer.parseInt(splitInfo[3]));
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(Student o) {
        return birthday.compareTo(o.birthday);
    }
}

class Birthday implements Comparable<Birthday> {
    private final int dayOfMonth;
    private final int month;
    private final int year;

    public Birthday(int dayOfMonth, int month, int year) {
        this.dayOfMonth = dayOfMonth;
        this.month = month;
        this.year = year;
    }

    @Override
    public int compareTo(Birthday o) {
        int result = Integer.compare(this.year, o.year);
        if (result != 0) {
            return result;
        }
        result = Integer.compare(this.month, o.month);
        if (result != 0) {
            return result;
        }
        return Integer.compare(this.dayOfMonth, o.dayOfMonth);
    }
}
~~~
