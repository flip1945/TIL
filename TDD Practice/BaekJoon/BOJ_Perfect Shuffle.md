# Perfect Shuffle (Silver 5)

### 문제 설명

A Perfect Shuffle of a deck of cards is executed by dividing the deck exactly in half, and then alternating cards from the two halves, starting with the top half. 

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images2/ps.png">

Given a deck of cards, perform a Perfect Shuffle. If there is an odd number of cards, give the top half split one more card than the bottom half.

출처 : https://www.acmicpc.net/problem/9492

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
        while (n != 0) {
            Deck deck = new Deck(inputCards(scanner, n));
            deck.shuffle().forEach(System.out::println);

            n = Integer.parseInt(scanner.nextLine());
        }
    }

    private static List<String> inputCards(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Deck {

    private final List<String> cards;

    public Deck(List<String> cards) {
        this.cards = cards;
    }

    public List<String> shuffle() {
        if (isEvenCards()) {
            return shuffleEvenCards(getFirstDeck(), getSecondDeck());
        }
        return shuffleOddCards(getFirstDeck(), getSecondDeck());
    }

    private boolean isEvenCards() {
        return cards.size() % 2 == 0;
    }

    private List<String> shuffleEvenCards(List<String> firstDeck, List<String> secondDeck) {
        List<String> result = new ArrayList<>();
        for (int i = 0; i < cards.size() / 2; i++) {
            result.add(firstDeck.get(i));
            result.add(secondDeck.get(i));
        }
        return result;
    }

    private List<String> getSecondDeck() {
        return cards.subList((cards.size() + 1) / 2, cards.size());
    }

    private List<String> getFirstDeck() {
        return cards.subList(0, (cards.size() + 1) / 2);
    }

    private List<String> shuffleOddCards(List<String> firstDeck, List<String> secondDeck) {
        List<String> result = shuffleEvenCards(firstDeck, secondDeck);
        result.add(getLastCardOfOdd(firstDeck));
        return result;
    }

    private String getLastCardOfOdd(List<String> firstDeck) {
        return firstDeck.get(firstDeck.size() - 1);
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

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideCards")
    @DisplayName("덱은 카드를 완벽히 섞는다.")
    void deckPerfectlyShufflesTheCards(List<String> cards, List<String> expected) {
        Deck sut = new Deck(cards);

        List<String> actual = sut.shuffle();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideCards() {
        return Stream.of(
                Arguments.of(List.of("ACE", "KING", "QUEEN", "JACK"), List.of("ACE", "QUEEN", "KING", "JACK")),
                Arguments.of(List.of("ONE", "TWO", "THREE", "FOUR"), List.of("ONE", "THREE", "TWO", "FOUR")),
                Arguments.of(List.of("SKIP", "DRAW-TWO", "REVERSE", "WILD", "WILD-DRAW-FOUR"), List.of("SKIP", "WILD",
                        "DRAW-TWO", "WILD-DRAW-FOUR", "REVERSE"))
        );
    }
}

class Deck {

    private final List<String> cards;

    public Deck(List<String> cards) {
        this.cards = cards;
    }

    public List<String> shuffle() {
        if (isEvenCards()) {
            return shuffleEvenCards(getFirstDeck(), getSecondDeck());
        }
        return shuffleOddCards(getFirstDeck(), getSecondDeck());
    }

    private boolean isEvenCards() {
        return cards.size() % 2 == 0;
    }

    private List<String> shuffleEvenCards(List<String> firstDeck, List<String> secondDeck) {
        List<String> result = new ArrayList<>();
        for (int i = 0; i < cards.size() / 2; i++) {
            result.add(firstDeck.get(i));
            result.add(secondDeck.get(i));
        }
        return result;
    }

    private List<String> getSecondDeck() {
        return cards.subList((cards.size() + 1) / 2, cards.size());
    }

    private List<String> getFirstDeck() {
        return cards.subList(0, (cards.size() + 1) / 2);
    }

    private List<String> shuffleOddCards(List<String> firstDeck, List<String> secondDeck) {
        List<String> result = shuffleEvenCards(firstDeck, secondDeck);
        result.add(getLastCardOfOdd(firstDeck));
        return result;
    }

    private String getLastCardOfOdd(List<String> firstDeck) {
        return firstDeck.get(firstDeck.size() - 1);
    }
}
~~~
