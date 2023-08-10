# 사과 (Bronze 3)

### 문제 설명

경상북도 특산품인 사과를 학생들에게 나눠주기 위해 여러 학교에 사과를 배정하였다. 배정된 사과 개수는 학교마다 다를 수 있고, 학생 수도 학교마다 다를 수 있다. 각 학교에서는 배정된 사과를 모든 학생들에게 똑같이 나눠주되, 남는 사과의 개수를 최소로 하려고 한다. (서로 다른 학교에 속한 학생이 받는 사과 개수는 다를 수 있다.)

예를 들어, 5개 학교의 학생 수와 배정된 사과 수가 다음과 같다고 하자.

|학교|	A|	B|	C|	D|	E|
|-|-|-|-|-|-|
|학생 수|	24|	13|	5|	23|	7|
|사과 개수|	52|	22|	53|	10|	70|

A 학교에서는 모든 학생에게 사과를 두 개씩 나눠주고 4개의 사과가 남게 된다. B 학교에서는 모든 학생에게 사과를 한 개씩 나눠주고 9개의 사과가 남게 된다. 비슷하게 C 학교에서는 3개의 사과가, D 학교에서는 10개의 사과가, E 학교에서는 0개의 사과가 남게 되어, 남는 사과의 총 수는 4+9+3+10+0 = 26이다. 

각 학교의 학생 수와 사과 개수가 주어졌을 때, 학생들에게 나눠주고 남는 사과의 총 개수를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/10833

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        Schools schools = new Schools(inputSchools(scanner, n));
        System.out.println(schools.getRemainApples());
    }

    private static List<String> inputSchools(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Schools {

    private final List<String> schools;

    public Schools(List<String> schools) {
        this.schools = schools;
    }

    public int getRemainApples() {
        return schools.stream()
                .map(School::from)
                .mapToInt(School::getCountOfRemainingApple)
                .sum();
    }
}

class School {

    public static final String DELIMITER = " ";
    public static final int COUNT_OF_STUDENTS_INDEX = 0;
    public static final int COUNT_OF_APPLES_INDEX = 1;

    private final int countOfStudents;
    private final int countOfApples;

    private School(int countOfStudents, int countOfApples) {
        this.countOfStudents = countOfStudents;
        this.countOfApples = countOfApples;
    }

    public static School from(String school) {
        String[] splitString = school.split(DELIMITER);
        return new School(Integer.parseInt(splitString[COUNT_OF_STUDENTS_INDEX]),
                Integer.parseInt(splitString[COUNT_OF_APPLES_INDEX]));
    }

    public int getCountOfRemainingApple() {
        return countOfApples % countOfStudents;
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
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSchools")
    @DisplayName("학교별 사과의 수와 학생 수가 주어지면 남은 사과의 총합을 반환한다.")
    void testGetRemainApples(List<String> schools, int expected) {
        Schools sut = new Schools(schools);

        int actual = sut.getRemainApples();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSchools() {
        return Stream.of(
                Arguments.of(List.of("1 1"), 0),
                Arguments.of(List.of("2 3"), 1),
                Arguments.of(List.of("4 3"), 3),
                Arguments.of(List.of("4 3", "2 3"), 4),
                Arguments.of(List.of("1 1", "1 1", "1 1"), 0),
                Arguments.of(List.of("24 52", "13 22", "5 53", "23 10", "7 70"), 26),
                Arguments.of(List.of("10 20", "5 5", "1 13"), 0)
        );
    }
}

class Schools {

    private final List<String> schools;

    public Schools(List<String> schools) {
        this.schools = schools;
    }

    public int getRemainApples() {
        return schools.stream()
                .map(School::from)
                .mapToInt(School::getCountOfRemainingApple)
                .sum();
    }
}

class School {

    public static final String DELIMITER = " ";
    public static final int COUNT_OF_STUDENTS_INDEX = 0;
    public static final int COUNT_OF_APPLES_INDEX = 1;

    private final int countOfStudents;
    private final int countOfApples;

    private School(int countOfStudents, int countOfApples) {
        this.countOfStudents = countOfStudents;
        this.countOfApples = countOfApples;
    }

    public static School from(String school) {
        String[] splitString = school.split(DELIMITER);
        return new School(Integer.parseInt(splitString[COUNT_OF_STUDENTS_INDEX]),
                Integer.parseInt(splitString[COUNT_OF_APPLES_INDEX]));
    }

    public int getCountOfRemainingApple() {
        return countOfApples % countOfStudents;
    }
}
~~~
