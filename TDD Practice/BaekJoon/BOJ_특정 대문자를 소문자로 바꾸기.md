# 특정 대문자를 소문자로 바꾸기 (Bronze 3)

### 문제 설명

알파벳 대소문자로 구성된 문자열 A가 주어진다. 한 개 이상의 알파벳 대문자가 공백으로 구분된 문자 목록 B가 주어진다. 문자 목록 B에는 중복된 대문자가 존재하지 않는다. 문자 목록 B에 존재하는 모든 대문자 b에 대하여, 문자열 A에 존재하는 대문자 b를 대응하는 소문자로 치환한 문자열을 C라고 하자. 입력으로 문자열 A와 문자 목록 B가 주어지면 문자열 C를 출력하자.

출처 : https://www.acmicpc.net/problem/26040

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String word = scanner.nextLine();
        String letterToChange = scanner.nextLine();

        ConvertStrategy convertStrategy = new LowercaseConvertStrategy(letterToChange);
        WordConverter wordConverter = new WordConverter(convertStrategy);
        System.out.println(wordConverter.convert(word));
    }
}

class WordConverter {

    private final ConvertStrategy convertStrategy;

    public WordConverter(ConvertStrategy convertStrategy) {
        this.convertStrategy = convertStrategy;
    }

    public String convert(String word) {
        StringBuilder result = new StringBuilder();
        for (char letter : word.toCharArray()) {
            result.append(convertStrategy.convert(letter));
        }
        return result.toString();
    }
}

interface ConvertStrategy {
    Character convert(Character letter);
}

class LowercaseConvertStrategy implements ConvertStrategy {

    private final Set<Character> charactersToLowercase;

    public LowercaseConvertStrategy(String letterToChange) {
        this.charactersToLowercase = getCharactersToLowercase(letterToChange);
    }

    private Set<Character> getCharactersToLowercase(String letterToChange) {
        Set<Character> lettersToChange = new HashSet<>();
        for (char c : letterToChange.toCharArray()) {
            lettersToChange.add(c);
        }
        return lettersToChange;
    }

    @Override
    public Character convert(Character letter) {
        return charactersToLowercase.contains(letter) ? Character.toLowerCase(letter) : letter;
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

import java.util.*;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideWords")
    @DisplayName("특정 대문자를 소문자로 변경한다.")
    void convertSomeLetterToLowercaseTest(String word, String letterToChange, String expected) {
        WordConverter sut = new WordConverter(new LowercaseConvertStrategy(letterToChange));

        String actual = sut.convert(word);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideWords() {
        return Stream.of(
                Arguments.of("abcD", "D", "abcd"),
                Arguments.of("AbcD", "D", "Abcd"),
                Arguments.of("AbcD", "A", "abcD"),
                Arguments.of("AbcD", "A D", "abcd"),
                Arguments.of("bcde", "A", "bcde"),
                Arguments.of("ABabC", "A", "aBabC"),
                Arguments.of("ABabC", "A B D", "ababC")
        );
    }
}

class WordConverter {

    private final ConvertStrategy convertStrategy;

    public WordConverter(ConvertStrategy convertStrategy) {
        this.convertStrategy = convertStrategy;
    }

    public String convert(String word) {
        StringBuilder result = new StringBuilder();
        for (char letter : word.toCharArray()) {
            result.append(convertStrategy.convert(letter));
        }
        return result.toString();
    }
}

interface ConvertStrategy {
    Character convert(Character letter);
}

class LowercaseConvertStrategy implements ConvertStrategy {

    private final Set<Character> charactersToLowercase;

    public LowercaseConvertStrategy(String letterToChange) {
        this.charactersToLowercase = getCharactersToLowercase(letterToChange);
    }

    private Set<Character> getCharactersToLowercase(String letterToChange) {
        Set<Character> lettersToChange = new HashSet<>();
        for (char c : letterToChange.toCharArray()) {
            lettersToChange.add(c);
        }
        return lettersToChange;
    }

    @Override
    public Character convert(Character letter) {
        return charactersToLowercase.contains(letter) ? Character.toLowerCase(letter) : letter;
    }
}
~~~
