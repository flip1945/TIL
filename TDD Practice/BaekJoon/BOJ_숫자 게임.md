# 숫자 게임 (Silver 5)

### 문제 설명

N명이 모여 숫자 게임을 하고자 한다. 각 사람에게는 1부터 10사이의 수가 적혀진 다섯 장의 카드가 주어진다. 그 중 세 장의 카드를 골라 합을 구한 후 일의 자리 수가 가장 큰 사람이 게임을 이기게 된다. 세 장의 카드가 (7, 8, 10)인 경우에는 합은 7+8+10 = 25가 되고 일의 자리 수는 5가 된다. 어떤 사람이 받은 카드가 (7, 5, 5, 4, 9)인 경우 (7, 4, 9)를 선택하면 합이 20이 되어 일의 자리 수는 0이 되고, (5, 5, 9)를 선택하면 합이 19가 되어 일의 자리 수는 9가 된다. 게임을 이기기 위해서는 세 장의 카드를 선택할 때 그 합의 일의 자리 수가 가장 크게 되도록 선택하여야 한다.

예를 들어, N=3일 때

* 1번 사람이 (7, 5, 5, 4, 9),
* 2번 사람이 (1, 1, 1, 1, 1),
* 3번 사람이 (2, 3, 3, 2, 10)의 

카드들을 받았을 경우, 세 수의 합에서 일의 자리 수가 가장 크게 되도록 세 수를 선택하면

* 1번 사람은 (5, 5, 9)에서 9,
* 2번 사람은 (1, 1, 1)에서 3,
* 3번 사람은 (2, 3, 3)에서 8의

결과를 각각 얻을 수 있으므로 첫 번째 사람이 이 게임을 이기게 된다.

N명에게 각각 다섯 장의 카드가 주어졌을 때, 세 장의 카드를 골라 합을 구한 후 일의 자리 수가 가장 큰 사람을 찾는 프로그램을 작성하시오. 가장 큰 수를 갖는 사람이 두 명 이상일 경우에는 번호가 가장 큰 사람의 번호를 출력한다.

출처 : https://www.acmicpc.net/problem/2303

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<List<Integer>> cards = inputCards(scanner, n);

        NumberGame numberGame = new NumberGame(new HandCalculator());
        System.out.println(numberGame.findWinningPlayer(cards));
    }

    private static List<List<Integer>> inputCards(Scanner scanner, int n) {
        List<List<Integer>> cards = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            List<Integer> hand = new ArrayList<>();
            for (int j = 0; j < 5; j++) {
                hand.add(scanner.nextInt());
            }
            cards.add(hand);
        }
        return cards;
    }
}

class NumberGame {
    private final HandCalculator handCalculator;

    public NumberGame(HandCalculator handCalculator) {
        this.handCalculator = handCalculator;
    }

    public int findWinningPlayer(List<List<Integer>> cards) {
        return getResultOfPlays(cards).stream()
                .sorted()
                .limit(1)
                .findFirst()
                .map(ResultOfPlay::getPlayerNumber)
                .orElse(0);
    }

    private List<ResultOfPlay> getResultOfPlays(List<List<Integer>> cards) {
        return IntStream.range(0, cards.size())
                .mapToObj(i -> new ResultOfPlay(i + 1, handCalculator.calculate(cards.get(i))))
                .collect(Collectors.toList());
    }
}

class HandCalculator {
    public int calculate(List<Integer> hand) {
        int result = 0;
        for (int i = 0; i < hand.size() - 2; i++) {
            result = Math.max(result, calculateOfTwoHand(hand, i));
        }
        return result;
    }

    private int calculateOfTwoHand(List<Integer> hand, int i) {
        int result = 0;
        for (int j = i + 1; j < hand.size() - 1; j++) {
            result = Math.max(result,calculateOfAllHand(hand, i, j));
        }
        return result;
    }

    private int calculateOfAllHand(List<Integer> hand, int i, int j) {
        int result = 0;
        for (int k = j + 1; k < hand.size(); k++) {
            result = Math.max(result, calculateHand(hand.get(i), hand.get(j), hand.get(k)));
        }
        return result;
    }

    private int calculateHand(int firstHand, int secondHand, int lastHand) {
        return (firstHand + secondHand + lastHand) % 10;
    }
}

class ResultOfPlay implements Comparable<ResultOfPlay> {
    private final int playerNumber;
    private final int point;

    public ResultOfPlay(int playerNumber, int point) {
        this.playerNumber = playerNumber;
        this.point = point;
    }

    public int getPlayerNumber() {
        return playerNumber;
    }

    @Override
    public int compareTo(ResultOfPlay o) {
        int result = Integer.compare(o.point, this.point);
        if (result != 0) {
            return result;
        }
        return Integer.compare(o.playerNumber, this.playerNumber);
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
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideCards")
    @DisplayName("숫자게임에서 이긴 사람을 반환한다.")
    void findWinningPlayerTest(List<List<Integer>> cards, int expected) {
        NumberGame sut = new NumberGame(new HandCalculator());

        int actual = sut.findWinningPlayer(cards);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCards() {
        return Stream.of(
                Arguments.of(List.of(List.of(7, 5, 5, 4, 9), List.of(1, 1, 1, 1, 1), List.of(2, 3, 3, 2, 10)), 1),
                Arguments.of(List.of(List.of(2, 3, 3, 2, 10), List.of(1, 1, 1, 1, 1), List.of(7, 5, 5, 4, 9)), 3),
                Arguments.of(List.of(List.of(2, 3, 3, 2, 10), List.of(7, 5, 5, 4, 9), List.of(1, 1, 1, 1, 1)), 2),
                Arguments.of(List.of(List.of(3, 10, 10, 10, 10), List.of(1, 1, 1, 1, 1), List.of(10, 10, 1, 10, 2)), 3)
        );
    }

    @ParameterizedTest
    @MethodSource("provideHands")
    @DisplayName("패의 점수를 반환한다.")
    void getMaxPointOfHandTest(List<Integer> card, int expected) {
        HandCalculator sut = new HandCalculator();

        int actual = sut.calculate(card);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideHands() {
        return Stream.of(
                Arguments.of(List.of(7, 5, 5, 4, 9), 9),
                Arguments.of(List.of(1, 1, 1, 1, 1), 3),
                Arguments.of(List.of(2, 3, 3, 2, 10), 8),
                Arguments.of(List.of(3, 3, 3, 1, 1), 9),
                Arguments.of(List.of(10, 10, 10, 10, 10), 0)
        );
    }
}

class NumberGame {
    private final HandCalculator handCalculator;

    public NumberGame(HandCalculator handCalculator) {
        this.handCalculator = handCalculator;
    }

    public int findWinningPlayer(List<List<Integer>> cards) {
        return getResultOfPlays(cards).stream()
                .sorted()
                .limit(1)
                .findFirst()
                .map(ResultOfPlay::getPlayerNumber)
                .orElse(0);
    }

    private List<ResultOfPlay> getResultOfPlays(List<List<Integer>> cards) {
        return IntStream.range(0, cards.size())
                .mapToObj(i -> new ResultOfPlay(i + 1, handCalculator.calculate(cards.get(i))))
                .collect(Collectors.toList());
    }
}

class HandCalculator {
    public int calculate(List<Integer> hand) {
        int result = 0;
        for (int i = 0; i < hand.size() - 2; i++) {
            result = Math.max(result, calculateOfTwoHand(hand, i));
        }
        return result;
    }

    private int calculateOfTwoHand(List<Integer> hand, int i) {
        int result = 0;
        for (int j = i + 1; j < hand.size() - 1; j++) {
            result = Math.max(result,calculateOfAllHand(hand, i, j));
        }
        return result;
    }

    private int calculateOfAllHand(List<Integer> hand, int i, int j) {
        int result = 0;
        for (int k = j + 1; k < hand.size(); k++) {
            result = Math.max(result, calculateHand(hand.get(i), hand.get(j), hand.get(k)));
        }
        return result;
    }

    private int calculateHand(int firstHand, int secondHand, int lastHand) {
        return (firstHand + secondHand + lastHand) % 10;
    }
}

class ResultOfPlay implements Comparable<ResultOfPlay> {
    private final int playerNumber;
    private final int point;

    public ResultOfPlay(int playerNumber, int point) {
        this.playerNumber = playerNumber;
        this.point = point;
    }

    public int getPlayerNumber() {
        return playerNumber;
    }

    @Override
    public int compareTo(ResultOfPlay o) {
        int result = Integer.compare(o.point, this.point);
        if (result != 0) {
            return result;
        }
        return Integer.compare(o.playerNumber, this.playerNumber);
    }
}
~~~
