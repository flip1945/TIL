# 럭비 클럽 (Bronze 4)

### 문제 설명

올 골드 럭비 클럽의 회원들은 성인부 또는 청소년부로 분류된다.

나이가 17세보다 많거나, 몸무게가 80kg 이상이면 성인부이다. 그 밖에는 모두 청소년부이다. 클럽 회원들을 올바르게 분류하라.

출처 : https://www.acmicpc.net/problem/2083

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String memberInfo = scanner.nextLine();
        while (!memberInfo.equals("# 0 0")) {
            RugbyMember rugbyMember = RugbyMember.from(memberInfo);
            System.out.println(rugbyMember.getNameAndDivision());
            memberInfo = scanner.nextLine();
        }
    }
}

class RugbyMember {

    public static final int NAME_INDEX = 0;
    public static final int AGE_INDEX = 1;
    public static final int WEIGHT_INDEX = 2;
    public static final String SENIOR = "Senior";
    public static final String JUNIOR = "Junior";
    public static final String DELIMITER = " ";
    public static final int SENIOR_AGE = 18;
    public static final int SENIOR_WEIGHT = 80;

    private final String name;
    private final int age;
    private final int weight;

    private RugbyMember(String name, int age, int weight) {
        this.name = name;
        this.age = age;
        this.weight = weight;
    }

    public static RugbyMember from(String memberInfo) {
        String[] splitInfo = memberInfo.split(DELIMITER);
        return new RugbyMember(splitInfo[NAME_INDEX], Integer.parseInt(splitInfo[AGE_INDEX]),
                Integer.parseInt(splitInfo[WEIGHT_INDEX]));
    }

    public String getNameAndDivision() {
        return name + DELIMITER + getDivision();
    }

    private String getDivision() {
        return isSenior() ? SENIOR : JUNIOR;
    }

    private boolean isSenior() {
        return age >= SENIOR_AGE || weight >= SENIOR_WEIGHT;
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
    @MethodSource("providePlayerInfo")
    @DisplayName("각 회원에 대해 이름과 부문를 반환해야 한다.")
    void shouldReturnMemberNameAndDivision(String memberInfo, String expected) {
        RugbyMember sut = RugbyMember.from(memberInfo);

        String actual = sut.getNameAndDivision();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> providePlayerInfo() {
        return Stream.of(
                Arguments.of("A 10 50", "A Junior"),
                Arguments.of("B 10 50", "B Junior"),
                Arguments.of("C 18 50", "C Senior"),
                Arguments.of("D 15 80", "D Senior"),
                Arguments.of("E 17 79", "E Junior"),
                Arguments.of("Joe 16 34", "Joe Junior"),
                Arguments.of("Bill 18 65", "Bill Senior"),
                Arguments.of("Billy 17 65", "Billy Junior"),
                Arguments.of("Sam 17 85", "Sam Senior")
        );
    }
}

class RugbyMember {

    public static final int NAME_INDEX = 0;
    public static final int AGE_INDEX = 1;
    public static final int WEIGHT_INDEX = 2;
    public static final String SENIOR = "Senior";
    public static final String JUNIOR = "Junior";
    public static final String DELIMITER = " ";
    public static final int SENIOR_AGE = 18;
    public static final int SENIOR_WEIGHT = 80;

    private final String name;
    private final int age;
    private final int weight;

    private RugbyMember(String name, int age, int weight) {
        this.name = name;
        this.age = age;
        this.weight = weight;
    }

    public static RugbyMember from(String memberInfo) {
        String[] splitInfo = memberInfo.split(DELIMITER);
        return new RugbyMember(splitInfo[NAME_INDEX], Integer.parseInt(splitInfo[AGE_INDEX]),
                Integer.parseInt(splitInfo[WEIGHT_INDEX]));
    }

    public String getNameAndDivision() {
        return name + DELIMITER + getDivision();
    }

    private String getDivision() {
        return isSenior() ? SENIOR : JUNIOR;
    }

    private boolean isSenior() {
        return age >= SENIOR_AGE || weight >= SENIOR_WEIGHT;
    }
}
~~~
