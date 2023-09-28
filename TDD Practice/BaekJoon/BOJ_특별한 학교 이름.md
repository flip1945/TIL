# 특별한 학교 이름 (Bronze 5)

### 문제 설명

GEC에는 여러 학교가 있다. 각 학교의 약칭과 정식 명칭은 다음과 같다.

* NLCS: North London Collegiate School
* BHA: Branksome Hall Asia
* KIS: Korea International School
* SJA: St. Johnsbury Academy

학교 이름을 좋아하는 규빈이는, 학교 이름을 짧게 부르는 것을 싫어하기 때문에, 각 학교의 약칭이 주어졌을 때 정식 명칭을 출력하는 프로그램을 만들기로 하였다.

각 학교의 약칭이 주어졌을 때, 정식 명칭을 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/27889

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String initial = scanner.nextLine();

        School school = School.from(initial);
        System.out.println(school.getName());
    }
}

class School {

    private final SchoolInitial initial;

    private School(SchoolInitial initial) {
        this.initial = initial;
    }

    public static School from(String initial) {
        return new School(SchoolInitial.from(initial));
    }

    public String getName() {
        return initial.getFullName();
    }
}

enum SchoolInitial {
    NLCS("North London Collegiate School"),
    BHA("Branksome Hall Asia"),
    KIS("Korea International School"),
    SJA("St. Johnsbury Academy");

    private final String fullName;

    SchoolInitial(String fullName) {
        this.fullName = fullName;
    }

    public static SchoolInitial from(String initial) {
        return Arrays.stream(values())
                .filter(schoolInitial -> schoolInitial.name().equals(initial))
                .findFirst()
                .orElseThrow(IllegalArgumentException::new);
    }

    public String getFullName() {
        return fullName;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.Arrays;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSchoolInitial")
    @DisplayName("학교는 학교 이니셜을 받으면 정식 학교 명칭을 반환한다.")
    void schoolReturnsFullSchoolNameGivenSchoolInitial(String schoolInitial, String expected) {
        School sut = School.from(schoolInitial);

        String actual = sut.getName();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSchoolInitial() {
        return Stream.of(
                Arguments.of("NLCS", "North London Collegiate School"),
                Arguments.of("BHA", "Branksome Hall Asia"),
                Arguments.of("KIS", "Korea International School"),
                Arguments.of("SJA", "St. Johnsbury Academy")
        );
    }

    @Test
    @DisplayName("학교는 올바르지 않은 학교 이니셜을 받으면 예외를 발생시킨다.")
    void schoolThrowsExceptionGivenWrongSchoolInitial() {
        String wrongSchoolInitial = "NONE";

        assertThrows(IllegalArgumentException.class, () -> School.from(wrongSchoolInitial));
    }
}

class School {

    private final SchoolInitial initial;

    private School(SchoolInitial initial) {
        this.initial = initial;
    }

    public static School from(String initial) {
        return new School(SchoolInitial.from(initial));
    }

    public String getName() {
        return initial.getFullName();
    }
}

enum SchoolInitial {
    NLCS("North London Collegiate School"),
    BHA("Branksome Hall Asia"),
    KIS("Korea International School"),
    SJA("St. Johnsbury Academy");

    private final String fullName;

    SchoolInitial(String fullName) {
        this.fullName = fullName;
    }

    public static SchoolInitial from(String initial) {
        return Arrays.stream(values())
                .filter(schoolInitial -> schoolInitial.name().equals(initial))
                .findFirst()
                .orElseThrow(IllegalArgumentException::new);
    }

    public String getFullName() {
        return fullName;
    }
}
~~~
