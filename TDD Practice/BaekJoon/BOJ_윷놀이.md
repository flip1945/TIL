# 윷놀이 (Bronze 3)

### 문제 설명

우리나라 고유의 윷놀이는 네 개의 윷짝을 던져서 배(0)와 등(1)이 나오는 숫자를 세어 도, 개, 걸, 윷, 모를 결정한다. 네 개 윷짝을 던져서 나온 각 윷짝의 배 혹은 등 정보가 주어질 때 도(배 한 개, 등 세 개), 개(배 두 개, 등 두 개), 걸(배 세 개, 등 한 개), 윷(배 네 개), 모(등 네 개) 중 어떤 것인지를 결정하는 프로그램을 작성하라.

출처 : https://www.acmicpc.net/problem/2490

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        YutGame yutGame = new YutGame();
        for (int i = 0; i < 3; i++) {
            List<Integer> yut = inputYut(scanner);
            System.out.println(yutGame.play(yut));
        }
    }

    private static List<Integer> inputYut(Scanner scanner) {
        return IntStream.range(0, 4)
                .mapToObj(n -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class YutGame {
    public String play(List<Integer> yut) {
        int sumOfYut = getSumOfYut(yut);
        switch (sumOfYut) {
            case 0: return "D";
            case 1: return "C";
            case 2: return "B";
            case 3: return "A";
            default: return "E";
        }
    }

    private int getSumOfYut(List<Integer> yut) {
        return yut.stream()
                .mapToInt(Integer::intValue)
                .sum();
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

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideYut")
    @DisplayName("윷놀이의 결과를 반환한다.")
    void yut_game_result_returns(List<Integer> yut, String expected) {
        YutGame sut = new YutGame();

        String actual = sut.play(yut);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideYut() {
        return Stream.of(
                Arguments.of(List.of(1, 1, 1, 0), "A"),
                Arguments.of(List.of(1, 0, 1, 0), "B"),
                Arguments.of(List.of(0, 0, 1, 0), "C"),
                Arguments.of(List.of(0, 0, 0, 0), "D"),
                Arguments.of(List.of(1, 1, 1, 1), "E"),
                Arguments.of(List.of(1, 0, 1, 1), "A")
        );
    }
}

class YutGame {
    public String play(List<Integer> yut) {
        int sumOfYut = getSumOfYut(yut);
        switch (sumOfYut) {
            case 0: return "D";
            case 1: return "C";
            case 2: return "B";
            case 3: return "A";
            default: return "E";
        }
    }

    private int getSumOfYut(List<Integer> yut) {
        return yut.stream()
                .mapToInt(Integer::intValue)
                .sum();
    }
}
~~~
