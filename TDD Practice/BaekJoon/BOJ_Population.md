# Population (Bronze 4)

### 문제 설명

In Emily’s country it is estimated that a person dies every 7 seconds, and a person is born every 4 seconds. Given a beginning population, estimate the population after a certain period of time.

출처 : https://www.acmicpc.net/problem/26561

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            Population population = new Population(scanner.nextInt());
            System.out.println(population.estimatedPopulation(scanner.nextInt()));
        }
    }
}

class Population {

    private static final int DIE_PER_SECOND = 7;
    private static final int BORN_PER_SECOND = 4;

    private final int beginningPopulation;

    public Population(int beginningPopulation) {
        this.beginningPopulation = beginningPopulation;
    }

    public int estimatedPopulation(int time) {
        return beginningPopulation - estimatedPersonOfDie(time) + estimatedPersonOfBorn(time);
    }

    private int estimatedPersonOfDie(int time) {
        return time / DIE_PER_SECOND;
    }

    private int estimatedPersonOfBorn(int time) {
        return time / BORN_PER_SECOND;
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

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideBeginningPopulationAndTime")
    @DisplayName("시작 인구와 시간이 주어지면 인구를 구한다.")
    void populationGetsEstimatedPopulationGivenBeginningPopulationAndTime(int beginningPopulation, int time, int expected) {
        Population sut = new Population(beginningPopulation);

        int actual = sut.estimatedPopulation(time);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideBeginningPopulationAndTime() {
        return Stream.of(
                Arguments.of(12, 14, 13),
                Arguments.of(530, 200, 552),
                Arguments.of(4786, 3543, 5165)
        );
    }
}

class Population {

    private static final int DIE_PER_SECOND = 7;
    private static final int BORN_PER_SECOND = 4;

    private final int beginningPopulation;

    public Population(int beginningPopulation) {
        this.beginningPopulation = beginningPopulation;
    }

    public int estimatedPopulation(int time) {
        return beginningPopulation - estimatedPersonOfDie(time) + estimatedPersonOfBorn(time);
    }

    private int estimatedPersonOfDie(int time) {
        return time / DIE_PER_SECOND;
    }

    private int estimatedPersonOfBorn(int time) {
        return time / BORN_PER_SECOND;
    }
}
~~~
