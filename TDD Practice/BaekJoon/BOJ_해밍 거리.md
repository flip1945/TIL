# 해밍 거리 (Bronze 2)

### 문제 설명

해밍 거리란 두 숫자의 서로 다른 자리수의 개수이다. 두 이진수가 주어졌을 때, 해밍 거리를 계산하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/3449

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            String bit1 = scanner.nextLine();
            String bit2 = scanner.nextLine();
            System.out.println(new HammingDistance(bit1, bit2));
        }
    }
}

class HammingDistance {

    private final String firstBit;
    private final String secondBit;
    private final int bitSize;

    public HammingDistance(String firstBit, String secondBit) {
        this.firstBit = firstBit;
        this.secondBit = secondBit;
        this.bitSize = getBitSize(firstBit, secondBit);
    }

    private int getBitSize(String firstBit, String secondBit) {
        if (firstBit.length() != secondBit.length()) {
            throw new IllegalArgumentException("두 비트의 사이즈는 같아야 합니다.");
        }
        return firstBit.length();
    }

    @Override
    public String toString() {
        return "Hamming distance is " + calculateHammingDistance() + ".";
    }

    private int calculateHammingDistance() {
        return (int) IntStream.range(0, bitSize)
                .filter(i -> firstBit.charAt(i) != secondBit.charAt(i))
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

import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideBits")
    @DisplayName("주어진 두 비트의 Hamming distance를 반환한다.")
    void getEasiestQuizTest(String bit1, String bit2, String expected) {
        HammingDistance sut = new HammingDistance(bit1, bit2);

        String actual = sut.toString();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideBits() {
        return Stream.of(
                Arguments.of("1", "0", "Hamming distance is 1."),
                Arguments.of("1", "1", "Hamming distance is 0."),
                Arguments.of("000", "000", "Hamming distance is 0."),
                Arguments.of("1111111100000000", "0000000011111111", "Hamming distance is 16."),
                Arguments.of("101", "000", "Hamming distance is 2.")
        );
    }
}

class HammingDistance {

    private final String firstBit;
    private final String secondBit;
    private final int bitSize;

    public HammingDistance(String firstBit, String secondBit) {
        this.firstBit = firstBit;
        this.secondBit = secondBit;
        this.bitSize = getBitSize(firstBit, secondBit);
    }

    private int getBitSize(String firstBit, String secondBit) {
        if (firstBit.length() != secondBit.length()) {
            throw new IllegalArgumentException("두 비트의 사이즈는 같아야 합니다.");
        }
        return firstBit.length();
    }

    @Override
    public String toString() {
        return "Hamming distance is " + calculateHammingDistance() + ".";
    }

    private int calculateHammingDistance() {
        return (int) IntStream.range(0, bitSize)
                .filter(i -> firstBit.charAt(i) != secondBit.charAt(i))
                .count();
    }
}
~~~
