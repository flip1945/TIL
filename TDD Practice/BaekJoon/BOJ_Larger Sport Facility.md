# Larger Sport Facility (Bronze 5)

### 문제 설명

In a lot of places in the world, elite universities come in pairs and their students like to challenge each other every year. In England, Oxford and Cambridge are famous for The Boat Race, an annual rowing race that opposes them. In Switzerland, students from Zürich and Lausanne battle in a famous annual ski challenge.

TelecomParisTech and Eurecom, two famous French schools, are also planning to organize a multi-sport tournament. They have already agreed on the choice of sports and the rules of the game, but the only point of disagreement is on where the contest should be hosted. Indeed, every school has been bragging for years about the wonderful sport facilities that they have.

At last, it was agreed that the competition would be hosted at the school which has the larger rectangular sports field. The only thing left is to determine which school this is: given the size of the fields, determine which school has the field with the larger surface.

출처 : https://www.acmicpc.net/problem/16099

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            SportFacilities sportFacilities = new SportFacilities(scanner.nextInt(), scanner.nextInt(),
                    scanner.nextInt(), scanner.nextInt());

            System.out.println(sportFacilities.largerFacilityName());
        }
    }
}

class SportFacilities {
    private static final String TIE_FACILITY_NAME = "Tie";
    private static final String FIRST_FACILITY_NAME = "TelecomParisTech";
    private static final String SECOND_FACILITY_NAME = "Eurecom";

    private final int lengthOfFirstFacility;
    private final int widthOfFirstFacility;
    private final int lengthOfSecondFacility;
    private final int widthOfSecondFacility;

    public SportFacilities(int lengthOfFirstFacility, int widthOfFirstFacility, int lengthOfSecondFacility,
                           int widthOfSecondFacility) {
        this.lengthOfFirstFacility = lengthOfFirstFacility;
        this.widthOfFirstFacility = widthOfFirstFacility;
        this.lengthOfSecondFacility = lengthOfSecondFacility;
        this.widthOfSecondFacility = widthOfSecondFacility;
    }

    public String largerFacilityName() {
        SportFacility firstFacility = new SportFacility(lengthOfFirstFacility, widthOfFirstFacility);
        SportFacility secondFacility = new SportFacility(lengthOfSecondFacility, widthOfSecondFacility);
        return getLargerFacilityName(firstFacility, secondFacility);
    }

    private String getLargerFacilityName(SportFacility firstFacility, SportFacility secondFacility) {
        if (firstFacility.getArea() == secondFacility.getArea()) {
            return TIE_FACILITY_NAME;
        }
        return (firstFacility.getArea() > secondFacility.getArea()) ? FIRST_FACILITY_NAME : SECOND_FACILITY_NAME;
    }
}

class SportFacility {
    private final long length;
    private final long width;

    public SportFacility(long length, long width) {
        this.length = length;
        this.width = width;
    }

    public long getArea() {
        return length * width;
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

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSportFacilityInfos")
    @DisplayName("두 운동 시설의 정보가 주어졌을 때 더 큰 시설의 이름을 반환한다.")
    void sportFacilitiesReturnsLargerSportFacilityName(int length1, int width1, int length2, int width2, String expected) {
        SportFacilities sut = new SportFacilities(length1, width1, length2, width2);

        String actual = sut.largerFacilityName();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSportFacilityInfos() {
        return Stream.of(
                Arguments.of(1, 1, 1, 2, "Eurecom"),
                Arguments.of(2, 1, 1, 1, "TelecomParisTech"),
                Arguments.of(2, 3, 1, 6, "Tie"),
                Arguments.of(3, 2, 4, 2, "Eurecom"),
                Arguments.of(536874368, 268792221, 598, 1204, "TelecomParisTech")
        );
    }
}

class SportFacilities {
    private static final String TIE_FACILITY_NAME = "Tie";
    private static final String FIRST_FACILITY_NAME = "TelecomParisTech";
    private static final String SECOND_FACILITY_NAME = "Eurecom";

    private final int lengthOfFirstFacility;
    private final int widthOfFirstFacility;
    private final int lengthOfSecondFacility;
    private final int widthOfSecondFacility;

    public SportFacilities(int lengthOfFirstFacility, int widthOfFirstFacility, int lengthOfSecondFacility,
                           int widthOfSecondFacility) {
        this.lengthOfFirstFacility = lengthOfFirstFacility;
        this.widthOfFirstFacility = widthOfFirstFacility;
        this.lengthOfSecondFacility = lengthOfSecondFacility;
        this.widthOfSecondFacility = widthOfSecondFacility;
    }

    public String largerFacilityName() {
        SportFacility firstFacility = new SportFacility(lengthOfFirstFacility, widthOfFirstFacility);
        SportFacility secondFacility = new SportFacility(lengthOfSecondFacility, widthOfSecondFacility);
        return getLargerFacilityName(firstFacility, secondFacility);
    }

    private String getLargerFacilityName(SportFacility firstFacility, SportFacility secondFacility) {
        if (firstFacility.getArea() == secondFacility.getArea()) {
            return TIE_FACILITY_NAME;
        }
        return (firstFacility.getArea() > secondFacility.getArea()) ? FIRST_FACILITY_NAME : SECOND_FACILITY_NAME;
    }
}

class SportFacility {
    private final long length;
    private final long width;

    public SportFacility(long length, long width) {
        this.length = length;
        this.width = width;
    }

    public long getArea() {
        return length * width;
    }
}
~~~
