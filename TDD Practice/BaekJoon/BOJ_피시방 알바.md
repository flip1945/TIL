# 피시방 알바 (Bronze 2)

### 문제 설명

세준이는 피시방에서 아르바이트를 한다. 세준이의 피시방에는 1번부터 100번까지 컴퓨터가 있다.

들어오는 손님은 모두 자기가 앉고 싶은 자리에만 앉고싶어한다. 따라서 들어오면서 번호를 말한다. 만약에 그 자리에 사람이 없으면 그 손님은 그 자리에 앉아서 컴퓨터를 할 수 있고, 사람이 있다면 거절당한다.

거절당하는 사람의 수를 출력하는 프로그램을 작성하시오. 자리는 맨 처음에 모두 비어있고, 어떤 사람이 자리에 앉으면 자리를 비우는 일은 없다.

출처 : https://www.acmicpc.net/problem/1453

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<Integer> seatNumbersToWant = inputSeatNumbers(scanner, n);

        InternetCafe internetCafe = new InternetCafe();
        System.out.println(internetCafe.getCountOfReject(seatNumbersToWant));
    }

    private static List<Integer> inputSeatNumbers(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class InternetCafe {

    private final boolean[] seats = new boolean[101];

    public int getCountOfReject(List<Integer> seatNumbersToWant) {
        int result = 0;
        for (int seatNumber : seatNumbersToWant) {
            result += increaseCountOfRejectOrNot(seatNumber);
            fillSeat(seatNumber);
        }
        return result;
    }

    private void fillSeat(int seatNumber) {
        if (!seats[seatNumber]) {
            seats[seatNumber] = true;
        }
    }

    private int increaseCountOfRejectOrNot(int seatNumber) {
        return seats[seatNumber] ? 1 : 0;
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
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideWaitingLines")
    @DisplayName("대기줄이 주어지면 거절당한 사람의 수를 반환해야 한다.")
    void shouldReturnCountOfReject(List<Integer> seatNumbersToWant, int expected) {
        InternetCafe sut = new InternetCafe();

        int actual = sut.getCountOfReject(seatNumbersToWant);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideWaitingLines() {
        return Stream.of(
                Arguments.of(List.of(1), 0),
                Arguments.of(List.of(1, 1), 1),
                Arguments.of(List.of(1, 2), 0),
                Arguments.of(List.of(1, 2, 2), 1),
                Arguments.of(List.of(1, 2, 2, 2), 2),
                Arguments.of(List.of(1, 2, 3), 0),
                Arguments.of(List.of(1, 1, 1, 1, 1), 4)
        );
    }
}

class InternetCafe {

    private final boolean[] seats = new boolean[101];

    public int getCountOfReject(List<Integer> seatNumbersToWant) {
        int result = 0;
        for (int seatNumber : seatNumbersToWant) {
            result += increaseCountOfRejectOrNot(seatNumber);
            fillSeat(seatNumber);
        }
        return result;
    }

    private void fillSeat(int seatNumber) {
        if (!seats[seatNumber]) {
            seats[seatNumber] = true;
        }
    }

    private int increaseCountOfRejectOrNot(int seatNumber) {
        return seats[seatNumber] ? 1 : 0;
    }
}
~~~
