# Yangjojang of The Year (Bronze 1)

### 문제 설명

입학 OT때 누구보다도 남다르게 놀았던 당신은 자연스럽게 1학년 과대를 역임하게 되었다.

타교와의 조인트 엠티를 기획하려는 당신은 근처에 있는 학교 중 어느 학교가 술을 가장 많이 먹는지 궁금해졌다.

학교별로 한 해동안 술 소비량이 주어질 때, 가장 술 소비가 많은 학교 이름을 출력하여라.

출처 : https://www.acmicpc.net/problem/11557

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        SchoolDrinkingCalculator calculator = new SchoolDrinkingCalculator();
        int t = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < t; i++) {
            int n = Integer.parseInt(scanner.nextLine());
            List<String> schoolAndDrinkings = IntStream.range(0, n).mapToObj(num -> scanner.nextLine()).collect(Collectors.toList());
            System.out.println(calculator.calculate(schoolAndDrinkings));
        }
    }
}

class SchoolDrinkingCalculator {
    public String calculate(List<String> schoolAndDrinkings) {
        return getSchools(schoolAndDrinkings).stream()
                .sorted()
                .map(School::getName)
                .limit(1)
                .findFirst()
                .orElse("");
    }

    private List<School> getSchools(List<String> schoolAndDrinkings) {
        return schoolAndDrinkings.stream()
                .map(School::new)
                .collect(Collectors.toList());
    }
}

class School implements Comparable<School>{
    private final String name;
    private final int amountOfDrinking;

    public School(String schoolAndAmountOfDrinking) {
        String[] split = schoolAndAmountOfDrinking.split(" ");
        this.name = split[0];
        this.amountOfDrinking = Integer.parseInt(split[1]);
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(School o) {
        return Integer.compare(o.amountOfDrinking, this.amountOfDrinking);
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

import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSchoolAndDrinking")
    @DisplayName("술 소비가 가장 많은 학교의 이름을 출력한다.")
    void it_returns_name_of_school_with_the_highest_alcohol_consumption(List<String> schoolAndDrinkings, String expected) {
        SchoolDrinkingCalculator sut = new SchoolDrinkingCalculator();

        String actual = sut.calculate(schoolAndDrinkings);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSchoolAndDrinking() {
        return Stream.of(
                Arguments.of(List.of("A 10", "B 20"), "B"),
                Arguments.of(List.of("A 10", "B 20", "C 30"), "C"),
                Arguments.of(List.of("A 0"), "A"),
                Arguments.of(List.of("Yonsei 10", "Korea 100000", "Ewha 20"), "Korea"),
                Arguments.of(List.of("Yonsei 1", "Korea 100000"), "Korea")
        );
    }
}

class SchoolDrinkingCalculator {
    public String calculate(List<String> schoolAndDrinkings) {
        return getSchools(schoolAndDrinkings).stream()
                .sorted()
                .map(School::getName)
                .limit(1)
                .findFirst()
                .orElse("");
    }

    private List<School> getSchools(List<String> schoolAndDrinkings) {
        return schoolAndDrinkings.stream()
                .map(School::new)
                .collect(Collectors.toList());
    }
}

class School implements Comparable<School>{
    private final String name;
    private final int amountOfDrinking;

    public School(String schoolAndAmountOfDrinking) {
        String[] split = schoolAndAmountOfDrinking.split(" ");
        this.name = split[0];
        this.amountOfDrinking = Integer.parseInt(split[1]);
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(School o) {
        return Integer.compare(o.amountOfDrinking, this.amountOfDrinking);
    }
}
~~~
