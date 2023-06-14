# 패리티 (Bronze 2)

### 문제 설명

1의 개수가 홀수개인 비트스트링을 "홀수 패리티"를 가지고 있다고 한다. 또, 짝수개인 경우에는 "짝수 패리티"를 가지고 있다고 한다. 또, 0도 짝수로 간주한다. 따라서, 1이 없는 비트 스트링은 짝수 패리티를 가지고 있다.

마지막 숫자가 지워진 비트 스트링이 주어지고, 이 비트 스트링의 패리티가 주어졌을 때, 마지막 숫자를 올바르게 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/4597

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String bit = scanner.nextLine();
        while (!bit.equals("#")) {
            ParityBit parityBit = new ParityBit(bit);
            System.out.println(parityBit.getBitString());
            bit = scanner.nextLine();
        }
    }
}

class ParityBit {

    public static final int BEGIN_INDEX = 0;
    private final String bit;
    private final Parity parity;

    public ParityBit(String bit) {
        int delimiterIndex = bit.length() - 1;
        this.bit = bit.substring(BEGIN_INDEX, delimiterIndex);
        this.parity = Parity.valueOf(bit.substring(delimiterIndex).toUpperCase());
    }

    public String getBitString() {
        return bit + parity.getParityString(getOneCount());
    }

    private int getOneCount() {
        return (int) bit.chars().filter(ch -> ch == '1').count();
    }
}

enum Parity {
    E((oneCount) -> (oneCount % 2 == 0) ? "0" : "1","짝수 패리티"),
    O((oneCount) -> (oneCount % 2 == 0) ? "1" : "0","홀수 패리티");

    private final Function<Integer, String> parityFunction;
    private final String description;

    Parity(Function<Integer, String> parityFunction, String description) {
        this.parityFunction = parityFunction;
        this.description = description;
    }

    public String getParityString(int oneCount) {
        return parityFunction.apply(oneCount);
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

import java.util.function.Function;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideBits")
    @DisplayName("패리티 비트를 이용해 올바른 비트로 바꾼 비트 스트링을 반환한다.")
    void parityBitTest(String bit, String expected) {
        ParityBit sut = new ParityBit(bit);

        String actual = sut.getBitString();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideBits() {
        return Stream.of(
                Arguments.of("1e", "11"),
                Arguments.of("1o", "10"),
                Arguments.of("101e", "1010"),
                Arguments.of("010010o", "0100101"),
                Arguments.of("000e", "0000"),
                Arguments.of("110100101o", "1101001010")
        );
    }
}

class ParityBit {

    public static final int BEGIN_INDEX = 0;
    private final String bit;
    private final Parity parity;

    public ParityBit(String bit) {
        int delimiterIndex = bit.length() - 1;
        this.bit = bit.substring(BEGIN_INDEX, delimiterIndex);
        this.parity = Parity.valueOf(bit.substring(delimiterIndex).toUpperCase());
    }

    public String getBitString() {
        return bit + parity.getParityString(getOneCount());
    }

    private int getOneCount() {
        return (int) bit.chars().filter(ch -> ch == '1').count();
    }
}

enum Parity {
    E((oneCount) -> (oneCount % 2 == 0) ? "0" : "1","짝수 패리티"),
    O((oneCount) -> (oneCount % 2 == 0) ? "1" : "0","홀수 패리티");

    private final Function<Integer, String> parityFunction;
    private final String description;

    Parity(Function<Integer, String> parityFunction, String description) {
        this.parityFunction = parityFunction;
        this.description = description;
    }

    public String getParityString(int oneCount) {
        return parityFunction.apply(oneCount);
    }
}
~~~
