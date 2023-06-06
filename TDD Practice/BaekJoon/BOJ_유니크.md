# 유니크 (Bronze 1)

### 문제 설명

상근이와 친구들은 MT에 가서 아래 설명과 같이 재미있는 게임을 할 것이다.

각 플레이어는 1이상 100 이하의 정수를 카드에 적어 제출한다. 각 플레이어는 자신과 같은 수를 쓴 사람이 없다면, 자신이 쓴 수와 같은 점수를 얻는다. 만약, 같은 수를 쓴 다른 사람이 있는 경우에는 점수를 얻을 수 없다.

상근이와 친구들은 이 게임을 3번 했다. 각 플레이어가 각각 쓴 수가 주어졌을 때, 3번 게임에서 얻은 총 점수를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/5533

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<List<Integer>> submits = inputSubmits(scanner, n);

        UniqueGame.calculate(submits).forEach(System.out::println);
    }

    private static List<List<Integer>> inputSubmits(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> IntStream.range(0, 3).mapToObj(j -> scanner.nextInt()).collect(Collectors.toList()))
                .collect(Collectors.toList());
    }
}

class UniqueGame {

    public static final int START_CARD_INDEX = 0;
    public static final int END_CARD_INDEX = 3;
    public static final int DEFAULT_POINT = 0;

    public static List<Integer> calculate(List<List<Integer>> submits) {
        return IntStream.range(0, submits.size())
                .mapToObj(player -> calculatePointOfPlayer(submits, player))
                .collect(Collectors.toList());
    }

    private static int calculatePointOfPlayer(List<List<Integer>> submits, int player) {
        return IntStream.range(START_CARD_INDEX, END_CARD_INDEX)
                .map(card -> calculatePoint(submits, player, card))
                .sum();
    }

    private static int calculatePoint(List<List<Integer>> submits, int player, int card) {
        int submitCard = submits.get(player).get(card);
        return isUnique(submits, player, card) ? submitCard : DEFAULT_POINT;
    }

    private static boolean isUnique(List<List<Integer>> submits, int player, int card) {
        int submitCard = submits.get(player).get(card);
        return IntStream.range(0, submits.size())
                .filter(otherPlayer -> otherPlayer != player)
                .noneMatch(otherPlayer -> Objects.equals(submitCard, submits.get(otherPlayer).get(card)));
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
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSubmits")
    @DisplayName("유니크 게임의 총점을 반환한다.")
    void it_returns_total_point_of_unique_game(List<List<Integer>> submits, List<Integer> expected) {
        List<Integer> actual = UniqueGame.calculate(submits);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSubmits() {
        return Stream.of(
                Arguments.of(List.of(List.of(1, 2, 3), List.of(1, 2, 3)), List.of(0, 0)),
                Arguments.of(List.of(List.of(1, 2, 4), List.of(1, 2, 3)), List.of(4, 3)),
                Arguments.of(List.of(List.of(1, 3, 4), List.of(1, 2, 3)), List.of(7, 5)),
                Arguments.of(List.of(List.of(1, 3, 4), List.of(2, 2, 3)), List.of(8, 7)),
                Arguments.of(List.of(List.of(100, 99, 98), List.of(100, 97, 92), List.of(63, 89, 63), List.of(99, 99, 99), List.of(89, 97, 98)), List.of(0, 92, 215, 198, 89)),
                Arguments.of(List.of(List.of(89, 92, 77), List.of(89, 92, 63), List.of(89, 63, 77)), List.of(0, 63, 63))
        );
    }
}

class UniqueGame {

    public static final int START_CARD_INDEX = 0;
    public static final int END_CARD_INDEX = 3;
    public static final int DEFAULT_POINT = 0;

    public static List<Integer> calculate(List<List<Integer>> submits) {
        return IntStream.range(0, submits.size())
                .mapToObj(player -> calculatePointOfPlayer(submits, player))
                .collect(Collectors.toList());
    }

    private static int calculatePointOfPlayer(List<List<Integer>> submits, int player) {
        return IntStream.range(START_CARD_INDEX, END_CARD_INDEX)
                .map(card -> calculatePoint(submits, player, card))
                .sum();
    }

    private static int calculatePoint(List<List<Integer>> submits, int player, int card) {
        int submitCard = submits.get(player).get(card);
        return isUnique(submits, player, card) ? submitCard : DEFAULT_POINT;
    }

    private static boolean isUnique(List<List<Integer>> submits, int player, int card) {
        int submitCard = submits.get(player).get(card);
        return IntStream.range(0, submits.size())
                .filter(otherPlayer -> otherPlayer != player)
                .noneMatch(otherPlayer -> Objects.equals(submitCard, submits.get(otherPlayer).get(card)));
    }
}
~~~
