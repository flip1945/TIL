# JOI와 IOI (Bronze 2)

### 문제 설명

입력으로 주어지는 문자열에서 연속으로 3개의 문자가 JOI 또는 IOI인 곳이 각각 몇 개 있는지 구하는 프로그램을 작성하시오. 문자열은 알파벳 대문자로만 이루어져 있다. 예를 들어, 아래와 같이 "JOIOIOI"에는 JOI가 1개, IOI가 2개 있다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/joioioi.png">

출처 : https://www.acmicpc.net/problem/5586

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        JoiAndIoiCounter counter = new JoiAndIoiCounter(scanner.nextLine());
        System.out.println(counter.getJoiCount());
        System.out.println(counter.getIoiCount());
    }
}

class JoiAndIoiCounter {

    private final int joiCount;
    private final int ioiCount;

    public JoiAndIoiCounter(String word) {
        this.joiCount = count("JOI", word);
        this.ioiCount = count("IOI", word);
    }

    private int count(String checkWord, String word) {
        return (int) IntStream.rangeClosed(0, word.length() - checkWord.length())
                .mapToObj(i -> getCheckableSubstring(word, i, checkWord.length()))
                .filter(check -> check.equals(checkWord))
                .count();
    }

    private String getCheckableSubstring(String word, int index, int checkWordSize) {
        return word.substring(index, index + checkWordSize);
    }

    public int getJoiCount() {
        return joiCount;
    }

    public int getIoiCount() {
        return ioiCount;
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

import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideWords")
    @DisplayName("JOI와 IOI의 숫자를 반환한다.")
    void getCountOfJoiAndIoiTest(String word, int joiExpected, int ioiExpected) {
        JoiAndIoiCounter actual = new JoiAndIoiCounter(word);

        assertEquals(joiExpected, actual.getJoiCount());
        assertEquals(ioiExpected, actual.getIoiCount());
    }

    private static Stream<Arguments> provideWords() {
        return Stream.of(
                Arguments.of("JOI", 1, 0),
                Arguments.of("IOI", 0, 1),
                Arguments.of("JOIOI", 1, 1),
                Arguments.of("JOIJOI", 2, 0),
                Arguments.of("JOIOIOI", 1, 2),
                Arguments.of("JOIOIOIOI", 1, 3),
                Arguments.of("JOIOIJOINXNXJIOIOIOJ", 2, 3),
                Arguments.of("J", 0, 0)
        );
    }
}

class JoiAndIoiCounter {

    private final int joiCount;
    private final int ioiCount;

    public JoiAndIoiCounter(String word) {
        this.joiCount = count("JOI", word);
        this.ioiCount = count("IOI", word);
    }

    private int count(String checkWord, String word) {
        return (int) IntStream.rangeClosed(0, word.length() - checkWord.length())
                .mapToObj(i -> getCheckableSubstring(word, i, checkWord.length()))
                .filter(check -> check.equals(checkWord))
                .count();
    }

    private String getCheckableSubstring(String word, int index, int checkWordSize) {
        return word.substring(index, index + checkWordSize);
    }

    public int getJoiCount() {
        return joiCount;
    }

    public int getIoiCount() {
        return ioiCount;
    }
}
~~~
