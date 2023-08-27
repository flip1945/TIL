# ROT13 (Bronze 1)

### 문제 설명

ROT13은 카이사르 암호의 일종으로 영어 알파벳을 13글자씩 밀어서 만든다.

예를 들어, "Baekjoon Online Judge"를 ROT13으로 암호화하면 "Onrxwbba Bayvar Whqtr"가 된다. ROT13으로 암호화한 내용을 원래 내용으로 바꾸려면 암호화한 문자열을 다시 ROT13하면 된다. 앞에서 암호화한 문자열 "Onrxwbba Bayvar Whqtr"에 다시 ROT13을 적용하면 "Baekjoon Online Judge"가 된다.

ROT13은 알파벳 대문자와 소문자에만 적용할 수 있다. 알파벳이 아닌 글자는 원래 글자 그대로 남아 있어야 한다. 예를 들어, "One is 1"을 ROT13으로 암호화하면 "Bar vf 1"이 된다.

문자열이 주어졌을 때, "ROT13"으로 암호화한 다음 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/11655

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Encryptor encryptor = new Encryptor(new Rot13Encrypt());

        System.out.println(encryptor.encrypt(scanner.nextLine()));
    }
}

class Encryptor {

    private final EncryptAlgorithm encryptAlgorithm;

    public Encryptor(EncryptAlgorithm encryptAlgorithm) {
        this.encryptAlgorithm = encryptAlgorithm;
    }

    public String encrypt(String sentence) {
        StringBuilder result = new StringBuilder();
        for (char letter : sentence.toCharArray()) {
            result.append(encryptAlgorithm.encryptLetter(letter));
        }
        return result.toString();
    }
}

interface EncryptAlgorithm {
    char encryptLetter(char letter);
}

class Rot13Encrypt implements EncryptAlgorithm {

    private static final char UPPERCASE_OFFSET = 'A';
    private static final char LOWERCASE_OFFSET = 'a';
    private static final int ROT13_SALT = 13;
    private static final int ALPHABET_SIZE = 26;

    public char encryptLetter(char letter) {
        if (Character.isAlphabetic(letter)) {
            return encryptLetterForRot13(letter);
        }
        return letter;
    }

    private char encryptLetterForRot13(char letter) {
        if (Character.isUpperCase(letter)) {
            return encryptLetterWithOffset(letter, UPPERCASE_OFFSET);
        }
        return encryptLetterWithOffset(letter, LOWERCASE_OFFSET);
    }

    private char encryptLetterWithOffset(char letter, char offset) {
        int sequenceOfAlphabet = letter - offset;
        int newSequenceOfAlphabet = (sequenceOfAlphabet + ROT13_SALT) % ALPHABET_SIZE;
        return (char) ((newSequenceOfAlphabet + offset));
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
    @MethodSource("provideSentence")
    @DisplayName("학점이 주어지면 평점을 반환한다.")
    void shouldEncryptToRot13Password(String sentence, String expected) {
        Encryptor sut = new Encryptor(new Rot13Encrypt());

        String actual = sut.encrypt(sentence);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSentence() {
        return Stream.of(
                Arguments.of("a", "n"),
                Arguments.of("b", "o"),
                Arguments.of("z", "m"),
                Arguments.of("ab", "no"),
                Arguments.of("a b", "n o"),
                Arguments.of("a b 1", "n o 1"),
                Arguments.of("A a", "N n"),
                Arguments.of("ABC", "NOP"),
                Arguments.of("Z", "M"),
                Arguments.of("Baekjoon Online Judge", "Onrxwbba Bayvar Whqtr"),
                Arguments.of("One is 1", "Bar vf 1")
        );
    }
}

class Encryptor {

    private final EncryptAlgorithm encryptAlgorithm;

    public Encryptor(EncryptAlgorithm encryptAlgorithm) {
        this.encryptAlgorithm = encryptAlgorithm;
    }

    public String encrypt(String sentence) {
        StringBuilder result = new StringBuilder();
        for (char letter : sentence.toCharArray()) {
            result.append(encryptAlgorithm.encryptLetter(letter));
        }
        return result.toString();
    }
}

interface EncryptAlgorithm {
    char encryptLetter(char letter);
}

class Rot13Encrypt implements EncryptAlgorithm {

    private static final char UPPERCASE_OFFSET = 'A';
    private static final char LOWERCASE_OFFSET = 'a';
    private static final int ROT13_SALT = 13;
    private static final int ALPHABET_SIZE = 26;

    public char encryptLetter(char letter) {
        if (Character.isAlphabetic(letter)) {
            return encryptLetterForRot13(letter);
        }
        return letter;
    }

    private char encryptLetterForRot13(char letter) {
        if (Character.isUpperCase(letter)) {
            return encryptLetterWithOffset(letter, UPPERCASE_OFFSET);
        }
        return encryptLetterWithOffset(letter, LOWERCASE_OFFSET);
    }

    private char encryptLetterWithOffset(char letter, char offset) {
        int sequenceOfAlphabet = letter - offset;
        int newSequenceOfAlphabet = (sequenceOfAlphabet + ROT13_SALT) % ALPHABET_SIZE;
        return (char) ((newSequenceOfAlphabet + offset));
    }
}
~~~
