# 관공... 어찌하여 목만 오셨소... (Bronze 4)

### 문제 설명

천하제일의 장수 관우도 결국 죽음을 맞이했다. 유비와 장비는 관우의 복수를 위해 $N$명의 용의자 중 관우를 죽인 범인을 찾으려 한다. 관우와 함께 있었던 장수의 말에 따르면 관우를 죽인 범인의 이름에는 S가 들어간다. 관우를 죽인 용의자 이름의 리스트에서 관우를 죽인 범인의 이름을 찾는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/30501

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

        Suspects suspects = new Suspects(inputSuspects(scanner, n));
        System.out.println(suspects.findCriminal());
    }

    private static List<String> inputSuspects(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Suspects {

    private static final String PARTIAL_CRIMINAL_NAME = "S";

    private final List<String> suspects;

    public Suspects(List<String> suspects) {
        this.suspects = suspects;
    }

    public String findCriminal() {
        return suspects.stream()
                .filter(this::isCriminal)
                .findFirst()
                .orElseThrow();
    }

    private boolean isCriminal(String suspect) {
        return suspect.contains(PARTIAL_CRIMINAL_NAME);
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

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSuspects")
    @DisplayName("용의자가 주어지면 범인을 찾는다.")
    void suspectsFindCriminalGivenSuspects(List<String> suspects, String expected) {
        Suspects sut = new Suspects(suspects);

        String actual = sut.findCriminal();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSuspects() {
        return Stream.of(
                Arguments.of(List.of("ABC", "SOSO"), "SOSO"),
                Arguments.of(List.of("ABC", "SUSA"), "SUSA"),
                Arguments.of(List.of("SIU", "ABC"), "SIU"),
                Arguments.of(List.of("ZHOUYU", "SUNQUAN", "ZOZO"), "SUNQUAN")
        );
    }
}

class Suspects {

    private static final String PARTIAL_CRIMINAL_NAME = "S";

    private final List<String> suspects;

    public Suspects(List<String> suspects) {
        this.suspects = suspects;
    }

    public String findCriminal() {
        return suspects.stream()
                .filter(this::isCriminal)
                .findFirst()
                .orElseThrow();
    }

    private boolean isCriminal(String suspect) {
        return suspect.contains(PARTIAL_CRIMINAL_NAME);
    }
}
~~~
