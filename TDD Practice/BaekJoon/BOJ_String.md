# String (Bronze 2)

### 문제 설명

It sometimes happens that a button on the computer keyboard sticks and then in the printed text there are more than one identical letters. For example, the word "piano" can change into "ppppppiaanooooo".

Your task is to write a program that corrects these errors: finds all the places within the given string, where identical letters follow each other and replaces them with one letter- that is, erases all the other identical letters and adds the remaining part of the string onto the end of the one remaining letter.

출처 : https://www.acmicpc.net/problem/7120

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String string = scanner.nextLine();
        StringCompressor stringCompressor = new StringCompressor(string);
        System.out.println(stringCompressor.compress());
    }
}

class StringCompressor {

    private final String string;

    public StringCompressor(String string) {
        this.string = string;
    }

    public String compress() {
        StringBuilder result = new StringBuilder();
        char previousLetter = ' ';
        for (char currentLetter : string.toCharArray()) {
            previousLetter = getPreviousLetter(result, previousLetter, currentLetter);
        }
        return result.toString();
    }

    private char getPreviousLetter(StringBuilder result, char prev, char ch) {
        if (prev != ch) {
            result.append(ch);
            return ch;
        }
        return prev;
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
    @MethodSource("provideString")
    @DisplayName("문자 압축기는 문자열을 압축한다.")
    void stringCompressorCompressTheString(String string, String expected) {
        StringCompressor sut = new StringCompressor(string);

        String actual = sut.compress();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideString() {
        return Stream.of(
                Arguments.of("aaaaa", "a"),
                Arguments.of("bbb", "b"),
                Arguments.of("aabb", "ab"),
                Arguments.of("ppppppiaanooooo", "piano")
        );
    }
}

class StringCompressor {

    private final String string;

    public StringCompressor(String string) {
        this.string = string;
    }

    public String compress() {
        StringBuilder result = new StringBuilder();
        char previousLetter = ' ';
        for (char currentLetter : string.toCharArray()) {
            previousLetter = getPreviousLetter(result, previousLetter, currentLetter);
        }
        return result.toString();
    }

    private char getPreviousLetter(StringBuilder result, char prev, char ch) {
        if (prev != ch) {
            result.append(ch);
            return ch;
        }
        return prev;
    }
}
~~~
