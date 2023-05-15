# 컵홀더 (Bronze 1)

### 문제 설명

십년이면 강산이 변한다.

강산이네 동네에 드디어 극장이 생겼고, 강산이는 극장에 놀러갔다. 매점에서 콜라를 산 뒤, 자리에 앉은 강산이는 큰 혼란에 빠졌다. 양쪽 컵홀더를 이미 옆 사람들이 차지했기 때문에 콜라를 꽂을 컵 홀더가 없었기 때문이다. 영화를 보는 내내 콜라를 손에 들고 있던 강산이는 극장에 다시 왔을 때는 꼭 콜라를 컵 홀더에 놓겠다는 다짐을 한 후 집에 돌아갔다.

극장의 한 줄에는 자리가 N개가 있다. 서로 인접한 좌석 사이에는 컵홀더가 하나씩 있고, 양 끝 좌석에는 컵홀더가 하나씩 더 있다. 또, 이 극장에는 커플석이 있다. 커플석 사이에는 컵홀더가 없다.

극장의 한 줄의 정보가 주어진다. 이때, 이 줄에 사람들이 모두 앉았을 때, 컵홀더에 컵을 꽂을 수 있는 최대 사람의 수를 구하는 프로그램을 작성하시오. 모든 사람은 컵을 한 개만 들고 있고, 자신의 좌석의 양 옆에 있는 컵홀더에만 컵을 꽂을 수 있다.

S는 일반 좌석, L은 커플석을 의미하며, L은 항상 두개씩 쌍으로 주어진다.

어떤 좌석의 배치가 SLLLLSSLL일때, 컵홀더를 *로 표시하면 아래와 같다.

~~~
*S*LL*LL*S*S*LL*
~~~

위의 예에서 적어도 두 명은 컵홀더를 사용할 수 없다.

출처 : https://www.acmicpc.net/problem/2810

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        CupHolderCalculator calculator = new CupHolderCalculator();

        System.out.println(calculator.calculate(scanner.nextLine()));
    }
}

class CupHolderCalculator {
    public static final String SINGLE_SEAT = "S";
    public static final String SINGLE_SEAT_WITH_CUP_HOLDER = "*S*";
    public static final String COUPLE_SEAT = "LL";
    public static final String COUPLE_SEAT_WITH_CUP_HOLDER = "*LL*";
    public static final String CUP_HOLDER = "*";
    public static final String DUPLICATED_CUP_HOLDER = "\\*+";

    public int calculate(String seats) {
        return getMinCupHolders(getSeatsWithCupHolders(seats), seats.length());
    }

    private String getSeatsWithCupHolders(String seats) {
        return seats.replaceAll(SINGLE_SEAT, SINGLE_SEAT_WITH_CUP_HOLDER)
                .replaceAll(COUPLE_SEAT, COUPLE_SEAT_WITH_CUP_HOLDER)
                .replaceAll(DUPLICATED_CUP_HOLDER, CUP_HOLDER);
    }

    private int getMinCupHolders(String seatWithCupHolders, int countOfSeats) {
        return Math.min(getCountOfCupHolders(seatWithCupHolders), countOfSeats);
    }

    private int getCountOfCupHolders(String seatWithCupHolders) {
        return (int) seatWithCupHolders.chars()
                .filter(ch -> ch == CUP_HOLDER.charAt(0))
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

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSeats")
    @DisplayName("꼽을 수 있는 컵홀더의 갯수를 반환한다.")
    void this_returns_the_count_of_available_cup_holders(String seats, int expected) {
        CupHolderCalculator sut = new CupHolderCalculator();

        int actual = sut.calculate(seats);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSeats() {
        return Stream.of(
                Arguments.of("S", 1),
                Arguments.of("SS", 2),
                Arguments.of("LLLL", 3),
                Arguments.of("SLLS", 4),
                Arguments.of("LLLLLL", 4),
                Arguments.of("SLLLLSSLL", 7)
        );
    }
}

class CupHolderCalculator {
    public static final String SINGLE_SEAT = "S";
    public static final String SINGLE_SEAT_WITH_CUP_HOLDER = "*S*";
    public static final String COUPLE_SEAT = "LL";
    public static final String COUPLE_SEAT_WITH_CUP_HOLDER = "*LL*";
    public static final String CUP_HOLDER = "*";
    public static final String DUPLICATED_CUP_HOLDER = "\\*+";

    public int calculate(String seats) {
        return getMinCupHolders(getSeatsWithCupHolders(seats), seats.length());
    }

    private String getSeatsWithCupHolders(String seats) {
        return seats.replaceAll(SINGLE_SEAT, SINGLE_SEAT_WITH_CUP_HOLDER)
                .replaceAll(COUPLE_SEAT, COUPLE_SEAT_WITH_CUP_HOLDER)
                .replaceAll(DUPLICATED_CUP_HOLDER, CUP_HOLDER);
    }

    private int getMinCupHolders(String seatWithCupHolders, int countOfSeats) {
        return Math.min(getCountOfCupHolders(seatWithCupHolders), countOfSeats);
    }

    private int getCountOfCupHolders(String seatWithCupHolders) {
        return (int) seatWithCupHolders.chars()
                .filter(ch -> ch == CUP_HOLDER.charAt(0))
                .count();
    }
}
~~~
