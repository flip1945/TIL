# 카드 바꿔치기 (Bronze 1)

### 문제 설명

범고래와 돌고래는 카드놀이를 좋아한다. 각 카드는 빨강 (R), 노랑 (Y), 파랑 (B) 중 하나의 색으로 칠해져 있고 0-9 사이의 숫자가 적혀있다. 색과 숫자가 같은 카드가 여러 장 있을 수도 있다.

최근 범고래는 모든 게임에서 졌고, 돌고래가 몰래 카드를 바꿔치기 한다는 의심을 하게 되었다.

범고래는 기억력이 매우 좋아서 카드놀이를 하기 전 n장의 카드와 카드놀이를 하면서 둘이 플레이 한 n장의 카드를 모두 기억하고 있다. 하지만 돌고래가 카드를 바꿔치기 했는지 아닌지 판단을 하는 능력은 부족하다.

예를 들어, 카드놀이를 하기 전에 n = 5장의 카드가 있었고, 이 카드는 아래와 같았다고 하자.

* R0
* B9
* R5
* Y3
* R2

카드놀이를 마치고 난 뒤 범고래가 기억하는 카드는 다음과 같다.

* R0
* B8
* R5
* Y3
* R2

이런 경우는 돌고래가 B9 카드 대신 B8 카드로 바꿔치기를 한 것이다.

돌고래가 카드 바꿔치기를 한 증거가 있는지 아닌지 판단하는 프로그램을 만들어보자.

출처 : https://www.acmicpc.net/problem/18766

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < t; i++) {
            int cardSize = Integer.parseInt(scanner.nextLine());
            List<String> originCards = inputCards(scanner);
            List<String> newCards = inputCards(scanner);

            Cards cards = new Cards(originCards);
            System.out.println(cards.isCheater(newCards));
        }
    }

    private static List<String> inputCards(Scanner scanner) {
        return Arrays.stream(scanner.nextLine().split(" "))
                .collect(Collectors.toList());
    }
}

class Cards {

    public static final String TRUE_STRING = "CHEATER";
    public static final String FALSE_STRING = "NOT CHEATER";

    private final List<String> cards;

    public Cards(List<String> cards) {
        this.cards = new ArrayList<>(cards);
    }

    public String isCheater(List<String> newCards) {
        return !getSortedCards(cards).equals(getSortedCards(newCards)) ? TRUE_STRING : FALSE_STRING;
    }

    private List<String> getSortedCards(List<String> cards) {
        return cards.stream()
                .sorted()
                .collect(Collectors.toList());
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
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideCards")
    @DisplayName("카드 바뀌치기를 했다면 CHEATER 아니라면 NOT CHEATER라고 반환한다.")
    void cheaterCheckTest(List<String> originCards, List<String> newCards, String expected) {
        Cards sut = new Cards(originCards);

        String actual = sut.isCheater(newCards);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCards() {
        return Stream.of(
                Arguments.of(List.of("R1"), List.of("R1"), "NOT CHEATER"),
                Arguments.of(List.of("R1"), List.of("R2"), "CHEATER"),
                Arguments.of(List.of("R1", "R2"), List.of("R1", "R2"), "NOT CHEATER"),
                Arguments.of(List.of("R2", "R1"), List.of("R1", "R2"), "NOT CHEATER"),
                Arguments.of(List.of("R2", "R1", "R1"), List.of("R1", "R2", "R2"), "CHEATER")
        );
    }
}

class Cards {

    public static final String TRUE_STRING = "CHEATER";
    public static final String FALSE_STRING = "NOT CHEATER";

    private final List<String> cards;

    public Cards(List<String> cards) {
        this.cards = new ArrayList<>(cards);
    }

    public String isCheater(List<String> newCards) {
        return !getSortedCards(cards).equals(getSortedCards(newCards)) ? TRUE_STRING : FALSE_STRING;
    }

    private List<String> getSortedCards(List<String> cards) {
        return cards.stream()
                .sorted()
                .collect(Collectors.toList());
    }
}
~~~
