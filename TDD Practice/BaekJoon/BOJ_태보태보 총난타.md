# 태보태보 총난타 (Bronze 2)

### 문제 설명

태보(TaeBo)란, 태권도와 복싱을 조합한 운동이다. 복싱의 공격 기술로는 민첩하게 앞주먹을 뻗으면서 가볍게 치는 잽, 옆으로 치는 펀치인 훅이 있다.

선풍적인 인기에 태보 강의를 들으며 태보를 마스터한 혜정이는 이제 펀치 속도가 워낙 빨라서 잽과 훅을 반복하다보면 잔상이 남는다.

얼굴의 왼편에 왼손의 잔상이, 오른편에는 오른손이 잔상이 남을 때 혜정이는 주먹을 몇 번 뻗었을까?

주먹의 잔상은 =로 시작하여 @로 끝나고, 잔상이 남지 않는 경우는 없다. 얼굴 형태가 (^0^) 꼴이고, 주먹의 잔상이 같은 곳에 위치하지 않는다.

출처 : https://www.acmicpc.net/problem/17249

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        TaeboHandCounter counter = new TaeboHandCounter();
        printTaeboHands(scanner.nextLine(), counter);
    }

    private static void printTaeboHands(String taebo, TaeboHandCounter counter) {
        counter.countBothHands(taebo)
                .forEach(hand -> System.out.print(hand + " "));
    }
}

class TaeboHandCounter {

    private static final String TAEBO_FACE = "0";
    private static final char TAEBO_HAND = '@';

    public List<Integer> countBothHands(String taebo) {
        String[] hands = getHands(taebo);
        int leftHand = getHandCount(hands[0]);
        int rightHand = getHandCount(hands[1]);
        return List.of(leftHand, rightHand);
    }

    private String[] getHands(String taebo) {
        return taebo.split(TAEBO_FACE);
    }

    private int getHandCount(String hand) {
        return (int) hand.chars()
                .filter(ch -> ch == TAEBO_HAND)
                .count();
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
    @MethodSource("provideTeabos")
    @DisplayName("왼손과 오른손의 태보 잔상의 수를 반환한다.")
    void getBothHandsWithTeaboTest(String taebo, List<Integer> expected) {
        TaeboHandCounter sut = new TaeboHandCounter();

        List<Integer> actual = sut.countBothHands(taebo);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideTeabos() {
        return Stream.of(
                Arguments.of("@=(^0^)==@", List.of(1, 1))
                , Arguments.of("@=(^0^)=@=@", List.of(1, 2))
                , Arguments.of("@=(^0^)============@@=@", List.of(1, 3))
                , Arguments.of("@===@==@=@==(^0^)==@=@===@", List.of(4, 3))
                , Arguments.of("@===(^0^)", List.of(1, 0))
                , Arguments.of("(^0^)", List.of(0, 0))
        );
    }
}

class TaeboHandCounter {

    private static final String TAEBO_FACE = "0";
    private static final char TAEBO_HAND = '@';

    public List<Integer> countBothHands(String taebo) {
        String[] hands = getHands(taebo);
        int leftHand = getHandCount(hands[0]);
        int rightHand = getHandCount(hands[1]);
        return List.of(leftHand, rightHand);
    }

    private String[] getHands(String taebo) {
        return taebo.split(TAEBO_FACE);
    }

    private int getHandCount(String hand) {
        return (int) hand.chars()
                .filter(ch -> ch == TAEBO_HAND)
                .count();
    }
}
~~~
