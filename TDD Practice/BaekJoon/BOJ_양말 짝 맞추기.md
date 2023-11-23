# 양말 짝 맞추기 (Bronze 4)

### 문제 설명

양말 $5$개가 주어집니다. 각 양말에는 숫자가 하나씩 쓰여있습니다. 같은 숫자가 쓰인 양말 두 개를 모으면 양말 한 쌍이 됩니다.

쌍을 만들 수 있는 양말을 두 개씩 두 쌍 빼면 남는 양말에 쓰인 숫자는 무엇인가요?

출처 : https://www.acmicpc.net/problem/28431

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        Socks socks = new Socks(inputSocks(scanner));
        System.out.println(socks.getMismatchedSock());
    }

    private static List<Integer> inputSocks(Scanner scanner) {
        return List.of(scanner.nextInt(), scanner.nextInt(), scanner.nextInt(), scanner.nextInt(),
                scanner.nextInt());
    }
}

class Socks {

    private final List<Integer> socks;

    public Socks(List<Integer> socks) {
        this.socks = socks;
    }

    public int getMismatchedSock() {
        return getSocksFrequencyMap(socks).entrySet().stream()
                .filter(this::isMismatched)
                .findFirst()
                .map(Map.Entry::getKey)
                .orElseThrow();
    }

    private Map<Integer, Long> getSocksFrequencyMap(List<Integer> socks) {
        return socks.stream()
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    }

    private boolean isMismatched(Map.Entry<Integer, Long> entry) {
        return entry.getValue() % 2 == 1;
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
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSocks")
    @DisplayName("양말 5개가 주어지면 짝이 없는 양말을 반환한다..")
    void socksReturnsMismatchedSock(List<Integer> socks, int expected) {
        Socks sut = new Socks(socks);

        int actual = sut.getMismatchedSock();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSocks() {
        return Stream.of(
                Arguments.of(List.of(1, 1, 2, 2, 3), 3),
                Arguments.of(List.of(1, 3, 2, 2, 3), 1),
                Arguments.of(List.of(6, 8, 6, 3, 8), 3),
                Arguments.of(List.of(9, 9, 9, 0, 0), 9)
        );
    }
}

class Socks {

    private final List<Integer> socks;

    public Socks(List<Integer> socks) {
        this.socks = socks;
    }

    public int getMismatchedSock() {
        return getSocksFrequencyMap(socks).entrySet().stream()
                .filter(this::isMismatched)
                .findFirst()
                .map(Map.Entry::getKey)
                .orElseThrow();
    }

    private Map<Integer, Long> getSocksFrequencyMap(List<Integer> socks) {
        return socks.stream()
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    }

    private boolean isMismatched(Map.Entry<Integer, Long> entry) {
        return entry.getValue() % 2 == 1;
    }
}
~~~
