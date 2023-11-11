# Split (Bronze 2)

### 문제 설명

In order to teach Mihai to write figures neatly, his teacher gave him for homework to write several numbers. Because he was rushing to finish his homework as quick as possible so that he can play on the computer, he wrote the numbers so close that some of them were attached.

When his mother looked on Mihai’s notebook, she noticed this thing and she thought of a challenge for the little boy. She writes a number with an even number of digits and Mihai should split this number in two equal parts and should write the two numbers which are formed after this move.

This thing will help Mihai learn how to write numbers neatly in a funny way.

Help Mihai find two numbers which form the number said by his mother, if we attach the second number at the end of the first one.

출처 : https://www.acmicpc.net/problem/21955

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String number = scanner.nextLine();
        TwoWord twoWord = new TwoWord(number);

        System.out.println(twoWord.splitNumber());
    }
}

class TwoWord {

    private static final String DELIMITER = " ";

    private final String word;

    public TwoWord(String word) {
        this.word = word;
    }

    public String splitNumber() {
        return getFirstNumber() + DELIMITER + getSecondNumber();
    }

    private String getFirstNumber() {
        return word.substring(0, getHalfIndex());
    }

    private String getSecondNumber() {
        return word.substring(getHalfIndex());
    }

    private int getHalfIndex() {
        return word.length() / 2;
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

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumber")
    @DisplayName("두 단어는 숫자를 둘로 나눈다.")
    void twoWordSplitTheNumbers(String number, String expected) {
        TwoWord sut = new TwoWord(number);

        String actual = sut.splitNumber();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideNumber() {
        return Stream.of(
                Arguments.of("2341", "23 41"),
                Arguments.of("238445", "238 445")
        );
    }
}

class TwoWord {

    private static final String DELIMITER = " ";

    private final String word;

    public TwoWord(String word) {
        this.word = word;
    }

    public String splitNumber() {
        return getFirstNumber() + DELIMITER + getSecondNumber();
    }

    private String getFirstNumber() {
        return word.substring(0, getHalfIndex());
    }

    private String getSecondNumber() {
        return word.substring(getHalfIndex());
    }

    private int getHalfIndex() {
        return word.length() / 2;
    }
}
~~~
