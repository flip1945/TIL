# Dyslexia (Bronze 3)

### 문제 설명

In the recent years children in Byteland have been hardly reading any books. This has a negative influence on the knowledge of orthography among Byteland residents. Teachers at schools do their best to change this situation. They organize many different tests and contests. The objective is to increase the knowledge of orthography among pupils. This, however, does not improve the situation much. Many children own dyslexia certificate, which allows them not to care about the orthographical mistakes that they make. Ministry of Education decided to counteract this situation. It has been decided that every owner of a dyslexia certificate has to prove that she or he is indeed dyslexic. There are so many children affected by dyslexia in Byteland that it is required to automate the process of validation. All children will have to rewrite a special set of texts on a computer. The number of mistakes made will make it possible to decide, whether the pupil is a dyslexic or not. Ministry of Education would like you to prepare a validating program for this test.

Write a program which:

* reads two texts from the standard input - the original one and the version rewritten by a pupil,
* determines the number of letters that were rewritten incorrectly,
* writes the result to the standard output.

출처 : https://www.acmicpc.net/problem/8371

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();
        String originWord = scanner.nextLine();
        String newWord = scanner.nextLine();

        Word word = new Word(originWord);
        System.out.println(word.getDifferentCount(newWord));
    }
}

class Word {

    private final String content;

    public Word(String content) {
        this.content = content;
    }

    public int getDifferentCount(String newWord) {
        return (int) IntStream.range(0, content.length())
                .filter(i -> content.charAt(i) != newWord.charAt(i))
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

import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideTwoWords")
    @DisplayName("두 단어에서 차이가 나는 글자의 개수를 반환한다.")
    void wordReturnsCountOfDifferent(String originWord, String newWord, int expected) {
        Word sut = new Word(originWord);

        int actual = sut.getDifferentCount(newWord);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideTwoWords() {
        return Stream.of(
                Arguments.of("a", "a", 0),
                Arguments.of("ab", "ac", 1),
                Arguments.of("ab", "cc", 2),
                Arguments.of("JASIOJESTDYSLEKTYKIEM", "JAsIOJSSTDXSIEKTYKLEM", 5)
        );
    }
}

class Word {

    private final String content;

    public Word(String content) {
        this.content = content;
    }

    public int getDifferentCount(String newWord) {
        return (int) IntStream.range(0, content.length())
                .filter(i -> content.charAt(i) != newWord.charAt(i))
                .count();
    }
}
~~~
