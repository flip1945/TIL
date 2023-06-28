# 할리갈리 (Bronze 2)

### 문제 설명

<img src="https://upload.acmicpc.net/d306bac4-bfc2-4dd3-a4be-9f832cdefd6c/-/preview/" width="320px">

《**할리갈리**》는 단추가 달린 종 하나와 과일이 그려진 카드들로 구성된 보드게임입니다.

카드에는 총 
$4$종류의 과일이 최대 
$5$개까지 그려져 있습니다. 그려진 과일의 종류는 딸기, 바나나, 라임, 그리고 자두입니다.

게임을 시작할 때 플레이어들은 카드 뭉치를 공평하게 나눠가지며 자신이 가진 카드를 전부 소모하면 패배합니다.

게임은 시작 플레이어가 본인의 카드 뭉치에서 카드 한 장을 공개하는 것으로 시작합니다. 이후 반시계 방향으로 돌아가며 본인의 카드를 한 장씩 공개합니다.

펼쳐진 카드들 중 한 종류 이상의 과일이 **정확히** 
$5$개 있는 경우 종을 눌러야 하며 가장 먼저 종을 누른 플레이어가 모든 카드를 모아 자신의 카드 뭉치 아래에 놓습니다. 종을 잘못 누른 경우 다른 모든 플레이어에게 카드를 한 장씩 나누어줘야 합니다.

《할리갈리》를 처음 해보는 한별이는 할리갈리 고수인 히나에게 이기기 위해 여러분에게 도움을 청했습니다. 한별이를 도와 펼쳐진 카드들의 목록이 주어졌을 때, 한별이가 종을 쳐야 하는지 알려주세요.

출처 : https://www.acmicpc.net/problem/27160

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        HalliGalli halliGalli = new HalliGalli(inputCards(scanner, n));
        System.out.println(halliGalli.ring());
    }

    private static List<String> inputCards(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class HalliGalli {

    public static final int RING_COUNT = 5;
    public static final String TRUE_STRING = "YES";
    public static final String FALSE_STRING = "NO";
    private final List<Card> cards;
    private final Map<Fruit, Integer> fruitsCounter;

    public HalliGalli(List<String> cards) {
        this.cards = cards.stream()
                .map(Card::new)
                .collect(Collectors.toList());
        this.fruitsCounter = new EnumMap<>(Fruit.class);
    }

    public String ring() {
        fillFruitCounter();
        return isRing();
    }

    private void fillFruitCounter() {
        cards.forEach(card -> fruitsCounter.merge(card.getFruit(), card.getCount(), Integer::sum));
    }

    private String isRing() {
        return fruitsCounter.containsValue(RING_COUNT) ? TRUE_STRING : FALSE_STRING;
    }
}

class Card {

    public static final String DELIMITER = " ";
    public static final int FRUIT_INDEX = 0;
    public static final int COUNT_INDEX = 1;
    private final Fruit fruit;
    private final int count;

    public Card(String card) {
        String[] split = card.split(DELIMITER);
        this.fruit = Fruit.valueOf(split[FRUIT_INDEX]);
        this.count = Integer.parseInt(split[COUNT_INDEX]);
    }

    public Fruit getFruit() {
        return fruit;
    }

    public int getCount() {
        return count;
    }
}

enum Fruit {
    STRAWBERRY, BANANA, LIME, PLUM
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
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideCards")
    @DisplayName("할리갈리 카드들을 확인해서 종을 쳐야 하면 YES를 아니라면 NO를 반환한다.")
    void halliGalliTest(List<String> cards, String expected) {
        HalliGalli sut = new HalliGalli(cards);

        String actual = sut.ring();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCards() {
        return Stream.of(
                Arguments.of(List.of("LIME 1"), "NO"),
                Arguments.of(List.of("LIME 5"), "YES"),
                Arguments.of(List.of("LIME 1", "BANANA 5"), "YES"),
                Arguments.of(List.of("LIME 1", "LIME 5"), "NO"),
                Arguments.of(List.of("LIME 2", "LIME 3"), "YES"),
                Arguments.of(List.of("LIME 2", "BANANA 1", "LIME 3"), "YES"),
                Arguments.of(List.of("LIME 2", "BANANA 1", "STRAWBERRY 4"), "NO"),
                Arguments.of(List.of("LIME 2", "BANANA 1", "STRAWBERRY 5"), "YES"),
                Arguments.of(List.of("LIME 2", "BANANA 1", "STRAWBERRY 2", "PLUM 3"), "NO")
        );
    }
}

class HalliGalli {

    public static final int RING_COUNT = 5;
    public static final String TRUE_STRING = "YES";
    public static final String FALSE_STRING = "NO";
    private final List<Card> cards;
    private final Map<Fruit, Integer> fruitsCounter;

    public HalliGalli(List<String> cards) {
        this.cards = cards.stream()
                .map(Card::new)
                .collect(Collectors.toList());
        this.fruitsCounter = new EnumMap<>(Fruit.class);
    }

    public String ring() {
        fillFruitCounter();
        return isRing();
    }

    private void fillFruitCounter() {
        cards.forEach(card -> fruitsCounter.merge(card.getFruit(), card.getCount(), Integer::sum));
    }

    private String isRing() {
        return fruitsCounter.containsValue(RING_COUNT) ? TRUE_STRING : FALSE_STRING;
    }
}

class Card {

    public static final String DELIMITER = " ";
    public static final int FRUIT_INDEX = 0;
    public static final int COUNT_INDEX = 1;
    private final Fruit fruit;
    private final int count;

    public Card(String card) {
        String[] split = card.split(DELIMITER);
        this.fruit = Fruit.valueOf(split[FRUIT_INDEX]);
        this.count = Integer.parseInt(split[COUNT_INDEX]);
    }

    public Fruit getFruit() {
        return fruit;
    }

    public int getCount() {
        return count;
    }
}

enum Fruit {
    STRAWBERRY, BANANA, LIME, PLUM
}
~~~
