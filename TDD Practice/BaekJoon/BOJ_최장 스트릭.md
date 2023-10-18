# 최장 스트릭 (Bronze 3)

### 문제 설명

solved.ac 사이트에는 문제를 며칠 연속으로 풀었는지 보여주는 지표가 있는데, 이를 스트릭이라고 한다. 총 $x$일 동안 매일 $1$문제 이상을 빠짐없이 풀었다면 스트릭 $x$일이라고 한다.

최장 스트릭은 총 
$N$일 기간 내에 달성한 스트릭 중 가장 큰 값을 의미한다. 
$N$일 동안의 푼 문제 수를 입력받아 최장 스트릭을 구하시오.

출처 : https://www.acmicpc.net/problem/29752

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        Streak streak = new Streak(inputNumbers(scanner, n));
        System.out.println(streak.getLongest());
    }

    private static List<Integer> inputNumbers(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class Streak {

    private final List<Integer> numbers;

    public Streak(List<Integer> numbers) {
        this.numbers = numbers;
    }

    public int getLongest() {
        int result = 0;
        int streak = 0;
        for (int i = 0; i < numbers.size(); i++) {
            streak = getStreak(streak, i);
            result = Math.max(result, streak);
        }
        return result;
    }

    private int getStreak(int streak, int index) {
        return isNotZeroCurrentNumber(index) ? streak + 1 : 0;
    }

    private boolean isNotZeroCurrentNumber(int index) {
        return numbers.get(index) != 0;
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
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideStreak")
    @DisplayName("최장 스트릭을 반환한다.")
    void streakReturnsTheLongestStreak(List<Integer> streak, int expected) {
        Streak sut = new Streak(streak);

        int actual = sut.getLongest();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideStreak() {
        return Stream.of(
                Arguments.of(List.of(1), 1),
                Arguments.of(List.of(1, 2), 2),
                Arguments.of(List.of(1, 0, 3), 1),
                Arguments.of(List.of(1, 2, 3, 4), 4),
                Arguments.of(List.of(0, 2, 3, 4), 3),
                Arguments.of(List.of(1, 0, 3, 4, 5), 3),
                Arguments.of(List.of(1, 4, 0, 1), 2),
                Arguments.of(List.of(1, 2, 0, 4, 5, 0, 7, 8, 9), 3),
                Arguments.of(List.of(0, 0, 1), 1),
                Arguments.of(List.of(0, 0, 0), 0)
        );
    }
}

class Streak {

    private final List<Integer> numbers;

    public Streak(List<Integer> numbers) {
        this.numbers = numbers;
    }

    public int getLongest() {
        int result = 0;
        int streak = 0;
        for (int i = 0; i < numbers.size(); i++) {
            streak = getStreak(streak, i);
            result = Math.max(result, streak);
        }
        return result;
    }

    private int getStreak(int streak, int index) {
        return isNotZeroCurrentNumber(index) ? streak + 1 : 0;
    }

    private boolean isNotZeroCurrentNumber(int index) {
        return numbers.get(index) != 0;
    }
}
~~~
