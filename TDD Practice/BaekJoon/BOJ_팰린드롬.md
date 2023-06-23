# 팰린드롬 (Bronze 2)

### 문제 설명

팰린드롬은 앞에서부터 읽을 때와 뒤에서부터 읽을 때가 똑같은 단어를 의미한다. 예를 들어, eve, eevee는 팰린드롬이고, eeve는 팰린드롬이 아니다. 단어가 주어졌을 때, 팰린드롬인지 아닌지 판단해보자.

출처 : https://www.acmicpc.net/problem/13235

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Palindrome palindrome = new Palindrome(scanner.nextLine());
        System.out.println(palindrome.isPalindrome());
    }
}

class Palindrome {

    private final OriginalReversedWord word;

    public Palindrome(String word) {
        this.word = OriginalReversedWord.from(word);
    }

    public boolean isPalindrome() {
        return Objects.equals(word.getOriginWord(), word.getReversedWord());
    }
}

class OriginalReversedWord {

    private final String originWord;
    private final String reversedWord;

    private OriginalReversedWord(String originWord) {
        this.originWord = originWord;
        this.reversedWord = getReversedWord(originWord);
    }

    private String getReversedWord(String originWord) {
        return new StringBuilder(originWord).reverse().toString();
    }

    public static OriginalReversedWord from(String word) {
        return new OriginalReversedWord(word);
    }

    public String getOriginWord() {
        return originWord;
    }

    public String getReversedWord() {
        return reversedWord;
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

import java.util.Objects;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePalindromeWords")
    @DisplayName("주어진 문자열이 팰리드롬이면 true를 반환한다.")
    void palindromeTest(String word) {
        Palindrome sut = new Palindrome(word);

        boolean actual = sut.isPalindrome();

        assertTrue(actual);
    }

    private static Stream<Arguments> providePalindromeWords() {
        return Stream.of(
                Arguments.of("a"),
                Arguments.of("aba"),
                Arguments.of("owo"),
                Arguments.of("bbbbbbbbbbb"),
                Arguments.of("uu")
        );
    }

    @ParameterizedTest
    @MethodSource("provideNotPalindromeWords")
    @DisplayName("주어진 문자열이 팰리드롬이 아니라면 false를 반환한다.")
    void notPalindromeTest(String word) {
        Palindrome sut = new Palindrome(word);

        boolean actual = sut.isPalindrome();

        assertFalse(actual);
    }

    private static Stream<Arguments> provideNotPalindromeWords() {
        return Stream.of(
                Arguments.of("ab"),
                Arguments.of("abca"),
                Arguments.of("zzzzzzzzo"),
                Arguments.of("slakdjfims")
        );
    }
}

class Palindrome {

    private final OriginalReversedWord word;

    public Palindrome(String word) {
        this.word = OriginalReversedWord.from(word);
    }

    public boolean isPalindrome() {
        return Objects.equals(word.getOriginWord(), word.getReversedWord());
    }
}

class OriginalReversedWord {

    private final String originWord;
    private final String reversedWord;

    private OriginalReversedWord(String originWord) {
        this.originWord = originWord;
        this.reversedWord = getReversedWord(originWord);
    }

    private String getReversedWord(String originWord) {
        return new StringBuilder(originWord).reverse().toString();
    }

    public static OriginalReversedWord from(String word) {
        return new OriginalReversedWord(word);
    }

    public String getOriginWord() {
        return originWord;
    }

    public String getReversedWord() {
        return reversedWord;
    }
}
~~~
