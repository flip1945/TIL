# Rust Study (Bronze 4)

### 문제 설명

취준생에서 직장인이 된 임스는 Rust 프로그래밍 언어를 공부하고자 책을 구매했다.

임스는 퇴근 후에 책을 정해진 페이지 수 이상으로 공부하기로 계획하였다.

임스가 공부하고자 한 일수와 공부하고자 계획한 페이지 수, 실제 공부한 페이지 수가 주어졌을 때 임스가 계획을 성실히 지킨 횟수를 구해 보자.

출처 : https://www.acmicpc.net/problem/30033

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<Integer> plans = inputPages(scanner, n);
        List<Integer> actualStudies = inputPages(scanner, n);

        StudyPlan studyPlan = new StudyPlan(plans, actualStudies);
        System.out.println(studyPlan.numberOfTimesOverStudies());
    }

    private static List<Integer> inputPages(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class StudyPlan {
    private final List<Integer> plans;
    private final List<Integer> actualStudies;

    public StudyPlan(List<Integer> plans, List<Integer> actualStudies) {
        this.plans = plans;
        this.actualStudies = actualStudies;
    }

    public int numberOfTimesOverStudies() {
        return (int) IntStream.range(0, plans.size())
                .filter(this::isOverStudy)
                .count();
    }

    private boolean isOverStudy(int i) {
        return actualStudies.get(i) >= plans.get(i);
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
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideStudyPlan")
    @DisplayName("공부 계획이 주어지면 계획한 페이지 수 이상으로 공부한 횟수를 반환한다.")
    void studyPlanReturnsNumberOfTimesStudiedMoreThanExpected(List<Integer> plans, List<Integer> actualStudies, int expected) {
        StudyPlan sut = new StudyPlan(plans, actualStudies);

        int actual = sut.numberOfTimesOverStudies();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideStudyPlan() {
        return Stream.of(
                Arguments.of(List.of(1), List.of(2), 1),
                Arguments.of(List.of(1, 2), List.of(2, 3), 2),
                Arguments.of(List.of(1, 2), List.of(2, 1), 1),
                Arguments.of(List.of(5, 6, 7, 8, 9), List.of(5, 5, 5, 10, 10), 3)
        );
    }
}

class StudyPlan {
    private final List<Integer> plans;
    private final List<Integer> actualStudies;

    public StudyPlan(List<Integer> plans, List<Integer> actualStudies) {
        this.plans = plans;
        this.actualStudies = actualStudies;
    }

    public int numberOfTimesOverStudies() {
        return (int) IntStream.range(0, plans.size())
                .filter(this::isOverStudy)
                .count();
    }

    private boolean isOverStudy(int i) {
        return actualStudies.get(i) >= plans.get(i);
    }
}
~~~
