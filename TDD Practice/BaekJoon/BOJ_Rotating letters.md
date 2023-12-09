# Rotating letters (Bronze 3)

### 문제 설명

An artist wants to construct a sign whose letters will rotate freely in the breeze. In order to do this, she must only use letters that are not changed by rotation of 180 degrees: I, O, S, H, Z, X, and N.

Write a program that reads a word and determines whether the word can be used on the sign.

출처 : https://www.acmicpc.net/problem/6750

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        RotatableSign rotatableSign = new RotatableSign(scanner.nextLine());
        System.out.println(rotatableSign.check());
    }
}

class RotatableSign {

    private static final String ROTATABLE_LETTERS = "IOSHZXN";
    private static final String ROTATABLE_SIGN = "YES";
    private static final String NOT_ROTATABLE_SIGN = "NO";

    private final String word;

    public RotatableSign(String word) {
        this.word = word;
    }

    public String check() {
        return isRotatableSign() ? ROTATABLE_SIGN : NOT_ROTATABLE_SIGN;
    }

    private boolean isRotatableSign() {
        return word.chars()
                .mapToObj(letter -> (char) letter)
                .map(String::valueOf)
                .allMatch(this::isRotatableLetter);
    }

    private boolean isRotatableLetter(String letter) {
        return ROTATABLE_LETTERS.contains(letter);
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
    @MethodSource("provideWord")
    @DisplayName("단어가 주어지면 회전 가능한 간판인지 확인한다.")
    void rotatableSignChecksRotatableSignGivenWord(String word, String expected) {
        RotatableSign sut = new RotatableSign(word);

        String actual = sut.check();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideWord() {
        return Stream.of(
                Arguments.of("S", "YES"),
                Arguments.of("A", "NO"),
                Arguments.of("IOS", "YES"),
                Arguments.of("IS", "YES"),
                Arguments.of("SHINS", "YES"),
                Arguments.of("ASHINS", "NO")
        );
    }
}

class RotatableSign {

    private static final String ROTATABLE_LETTERS = "IOSHZXN";
    private static final String ROTATABLE_SIGN = "YES";
    private static final String NOT_ROTATABLE_SIGN = "NO";

    private final String word;

    public RotatableSign(String word) {
        this.word = word;
    }

    public String check() {
        return isRotatableSign() ? ROTATABLE_SIGN : NOT_ROTATABLE_SIGN;
    }

    private boolean isRotatableSign() {
        return word.chars()
                .mapToObj(letter -> (char) letter)
                .map(String::valueOf)
                .allMatch(this::isRotatableLetter);
    }

    private boolean isRotatableLetter(String letter) {
        return ROTATABLE_LETTERS.contains(letter);
    }
}
~~~
