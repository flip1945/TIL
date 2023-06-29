# 문자가 몇갤까 (Bronze 2)

### 문제 설명

"The quick brown fox jumped over the lazy dogs."

이 문장은 모든 알파벳이 적어도 한 번은 나오는 문장으로 유명하다. 즉 26개의 서로 다른 문자를 갖고 있는 것이다.

각 케이스마다 문장에서 공백, 숫자, 특수 문자를 제외하고 얼마나 다양한 알파벳이 나왔는지를 구하면 된다. 대소문자는 하나의 문자로 처리한다. ex) 'A' == 'a'

출처 : https://www.acmicpc.net/problem/7600

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String sentence = scanner.nextLine();
        AlphabetCounter counter = new AlphabetCounter();

        while (!"#".equals(sentence)) {
            System.out.println(counter.count(sentence));
            sentence = scanner.nextLine();
        }
    }
}

class AlphabetCounter {

    public int count(String sentence) {
        return (int) sentence.chars()
                .filter(Character::isAlphabetic)
                .map(Character::toUpperCase)
                .distinct()
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

import java.util.*;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSentences")
    @DisplayName("대소문자를 구분하지 않고 문장안의 알파벳 개수를 반환한다.")
    void alphabetCounterTest(String sentence, int expected) {
        AlphabetCounter sut = new AlphabetCounter();

        int actual = sut.count(sentence);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSentences() {
        return Stream.of(
                Arguments.of("a", 1),
                Arguments.of("ab", 2),
                Arguments.of("aab", 2),
                Arguments.of("a b", 2),
                Arguments.of("a A", 1),
                Arguments.of("AAA", 1),
                Arguments.of("The quick brown fox jumped over the lazy dogs.", 26),
                Arguments.of("New Zealand Programming Contest.", 16),
                Arguments.of("2 + 2 = 4", 0)
        );
    }
}

class AlphabetCounter {

    public int count(String sentence) {
        return (int) sentence.chars()
                .filter(Character::isAlphabetic)
                .map(Character::toUpperCase)
                .distinct()
                .count();
    }
}
~~~
