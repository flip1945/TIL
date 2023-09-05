# 뉴비의 기준은 뭘까? (Bronze 4)

### 문제 설명

2020 INPC는 IGRUS 뉴비들을 위해 열리는 대회입니다. 하지만 영수 할아버지나 인용 할아버지와 같이 14학번이지만 마음만은 뉴비인 어르신들 때문에 대회장이 TLE들의 파티가 되자 뉴비의 기준을 정의하기로 하였습니다.

INPC 운영진들은 고심 끝에 뉴비를 1학년 혹은 2학년인 학생으로 정의 내렸고 뉴비를 정의하는 김에 올드비와 TLE도 정의 내리기로 하였습니다. 올드비는 N학년 이하이면서 뉴비가 아닌 학생으로 정의하기로 하였고 TLE은 뉴비도 아니고 올드비도 아닌 학생으로 정의하였습니다.

N과 M이 주어졌을 때, M학년이 뉴비인지 올드비인지 TLE인지 구별해 주세요.

출처 : https://www.acmicpc.net/problem/19944

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Student student = new Student(scanner.nextInt(), scanner.nextInt());
        System.out.println(student.getStudentType());
    }
}

class Student {

    private static final String NEWBIE = "NEWBIE!";
    private static final String OLDBIE = "OLDBIE!";
    private static final String TLE = "TLE!";
    private static final int NEWBIE_GRADE_1 = 1;
    private static final int NEWBIE_GRADE_2 = 2;

    private final int criteria;
    private final int grade;

    public Student(int criteria, int grade) {
        this.criteria = criteria;
        this.grade = grade;
    }

    public String getStudentType() {
        if (isNewbie()) {
            return NEWBIE;
        }
        return checkOldbieOrTle();
    }

    private boolean isNewbie() {
        return grade == NEWBIE_GRADE_1 || grade == NEWBIE_GRADE_2;
    }

    private String checkOldbieOrTle() {
        if (isOldbie()) {
            return OLDBIE;
        }
        return TLE;
    }

    private boolean isOldbie() {
        return grade <= criteria;
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
    @MethodSource("provideCriteriaAndGrade")
    @DisplayName("뉴비인지 올드비인지 TLE인지 구분해야 한다.")
    void shouldReturnStudentType(int criteria, int grade, String expected) {
        Student sut = new Student(criteria, grade);

        String actual = sut.getStudentType();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCriteriaAndGrade() {
        return Stream.of(
                Arguments.of(3, 1, "NEWBIE!"),
                Arguments.of(3, 2, "NEWBIE!"),
                Arguments.of(3, 3, "OLDBIE!"),
                Arguments.of(5, 3, "OLDBIE!"),
                Arguments.of(5, 4, "OLDBIE!"),
                Arguments.of(5, 5, "OLDBIE!"),
                Arguments.of(3, 5, "TLE!"),
                Arguments.of(0, 5, "TLE!"),
                Arguments.of(2, 3, "TLE!")
        );
    }
}

class Student {

    private static final String NEWBIE = "NEWBIE!";
    private static final String OLDBIE = "OLDBIE!";
    private static final String TLE = "TLE!";
    private static final int NEWBIE_GRADE_1 = 1;
    private static final int NEWBIE_GRADE_2 = 2;

    private final int criteria;
    private final int grade;

    public Student(int criteria, int grade) {
        this.criteria = criteria;
        this.grade = grade;
    }

    public String getStudentType() {
        if (isNewbie()) {
            return NEWBIE;
        }
        return checkOldbieOrTle();
    }

    private boolean isNewbie() {
        return grade == NEWBIE_GRADE_1 || grade == NEWBIE_GRADE_2;
    }

    private String checkOldbieOrTle() {
        if (isOldbie()) {
            return OLDBIE;
        }
        return TLE;
    }

    private boolean isOldbie() {
        return grade <= criteria;
    }
}
~~~
