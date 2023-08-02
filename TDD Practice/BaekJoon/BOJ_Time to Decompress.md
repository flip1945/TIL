# Time to Decompress (Bronze 3)

### 문제 설명

You and your friend have come up with a way to send messages back and forth.

Your friend can encode a message to you by writing down a positive integer N and a symbol. You can decode that message by writing out that symbol N times in a row on one line.

Given a message that your friend has encoded, decode it.

출처 : https://www.acmicpc.net/problem/17010

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            Compression compression = Compression.from(scanner.nextLine());
            System.out.println(compression.decompress());
        }
    }
}

class Compression {

    public static final String DELIMITER = " ";
    public static final int COUNT_INDEX = 0;
    public static final int CHARACTER_INDEX = 1;

    private final int countOfCompress;
    private final String characterOfCompress;

    private Compression(int countOfCompress, String characterOfCompress) {
        this.countOfCompress = countOfCompress;
        this.characterOfCompress = characterOfCompress;
    }

    public static Compression from(String compressedString) {
        String[] split = compressedString.split(DELIMITER);
        return new Compression(Integer.parseInt(split[COUNT_INDEX]), split[CHARACTER_INDEX]);
    }

    public String decompress() {
        return characterOfCompress.repeat(countOfCompress);
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
    @MethodSource("provideExpressions")
    @DisplayName("압축을 해제한 결과를 반환한다.")
    void decompressTest(String compressedString, String expected) {
        Compression sut = Compression.from(compressedString);

        String actual = sut.decompress();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideExpressions() {
        return Stream.of(
                Arguments.of("1 x", "x"),
                Arguments.of("1 a", "a"),
                Arguments.of("2 a", "aa"),
                Arguments.of("9 +", "+++++++++"),
                Arguments.of("3 -", "---"),
                Arguments.of("12 A", "AAAAAAAAAAAA"),
                Arguments.of("2 X", "XX")
        );
    }
}

class Compression {

    public static final String DELIMITER = " ";
    public static final int COUNT_INDEX = 0;
    public static final int CHARACTER_INDEX = 1;

    private final int countOfCompress;
    private final String characterOfCompress;

    private Compression(int countOfCompress, String characterOfCompress) {
        this.countOfCompress = countOfCompress;
        this.characterOfCompress = characterOfCompress;
    }

    public static Compression from(String compressedString) {
        String[] split = compressedString.split(DELIMITER);
        return new Compression(Integer.parseInt(split[COUNT_INDEX]), split[CHARACTER_INDEX]);
    }

    public String decompress() {
        return characterOfCompress.repeat(countOfCompress);
    }
}
~~~
