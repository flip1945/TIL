# Odd Man Out (Large) (Silver 5)

### 문제 설명

You are hosting a party with G guests and notice that there is an odd number of guests! When planning the party you deliberately invited only couples and gave each couple a unique number C on their invitation. You would like to single out whoever came alone by asking all of the guests for their invitation numbers.

출처 : https://www.acmicpc.net/problem/12596

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = scanner.nextInt();
        for (int i = 1; i <= t; i++) {
            int n = scanner.nextInt();
            List<Integer> guestNumbers = IntStream.range(0, n)
                    .mapToObj(j -> scanner.nextInt())
                    .collect(Collectors.toList());

            Party party = new Party(guestNumbers);
            System.out.println("Case #" + i + ": " + party.aloneGuest());
        }
    }
}

class Party {

    private static final int ALONE_GUEST_COUNT = 1;

    private final List<Integer> guestNumbers;

    public Party(List<Integer> guestNumbers) {
        this.guestNumbers = guestNumbers;
    }

    public int aloneGuest() {
        return getAloneGuestNumber(getGuestCounter(guestNumbers));
    }

    private Map<Integer, Integer> getGuestCounter(List<Integer> guestNumbers) {
        return guestNumbers.stream()
                .collect(Collectors.toMap(guestNumber -> guestNumber, guestNumber -> 1, Integer::sum));
    }

    private int getAloneGuestNumber(Map<Integer, Integer> counter) {
        for (Map.Entry<Integer, Integer> guest : counter.entrySet()) {
            if (guest.getValue() == ALONE_GUEST_COUNT) {
                return guest.getKey();
            }
        }
        throw new IllegalStateException();
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
import java.util.Map;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideGuests")
    @DisplayName("혼자 온 손님의 번호를 반환한다.")
    void partyReturnsAloneGuestNumber(List<Integer> guestNumbers, int expected) {
        Party sut = new Party(guestNumbers);

        int actual = sut.aloneGuest();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideGuests() {
        return Stream.of(
                Arguments.of(List.of(1, 2147483647, 2147483647), 1),
                Arguments.of(List.of(1, 1, 2147483647), 2147483647),
                Arguments.of(List.of(3, 4, 7, 4, 3), 7),
                Arguments.of(List.of(2, 10, 2, 10, 5), 5)
        );
    }
}

class Party {

    private static final int ALONE_GUEST_COUNT = 1;

    private final List<Integer> guestNumbers;

    public Party(List<Integer> guestNumbers) {
        this.guestNumbers = guestNumbers;
    }

    public int aloneGuest() {
        return getAloneGuestNumber(getGuestCounter(guestNumbers));
    }

    private Map<Integer, Integer> getGuestCounter(List<Integer> guestNumbers) {
        return guestNumbers.stream()
                .collect(Collectors.toMap(guestNumber -> guestNumber, guestNumber -> 1, Integer::sum));
    }

    private int getAloneGuestNumber(Map<Integer, Integer> counter) {
        for (Map.Entry<Integer, Integer> guest : counter.entrySet()) {
            if (guest.getValue() == ALONE_GUEST_COUNT) {
                return guest.getKey();
            }
        }
        throw new IllegalStateException();
    }
}
~~~
