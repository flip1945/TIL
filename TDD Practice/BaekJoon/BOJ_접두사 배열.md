# 접두사 배열 (Bronze 1)

### 문제 설명

접미사 배열(suffix array)이란, 어떤 문자열의 모든 접미사를 사전 순으로 정렬한 뒤, 각 접미사의 시작 위치를 기록한 배열을 의미한다. 예를 들어 'banana' 라는 문자열에 대해 접미사 배열을 구한다면 아래와 같다

1. 문자열의 모든 접미사는 아래와 같다.
    * banana, anana, nana, ana, na, a
2. 위 접미사들을 사전 순으로 정렬하면 아래와 같다.
    * a, ana, anana, banana, na, nana
3. 각 접미사의 원래 문자열에서의 시작 인덱스를 기록하면 아래와 같다.
    *  5, 3, 1, 0, 4, 2

따라서 문자열 'banana'의 접미사 배열은 { 5, 3, 1, 0, 4, 2 } 가 된다.

연세대학교의 PS 동아리 모르고리즘 회원 택희와 남규는 문자열 문제 하나를 같이 풀어보고 있었다. 다음은 그 과정에서 있었던 대화의 일부를 발췌한 것이다.

* 택희 : 이거 그냥 suffix array 구해놓고 풀면 되겠는데?
* 남규 : suffix array면.. 접미사 배열 구하고 뒤집으면 되나?
* 택희 : ??
* 남규 : ??
* 택희 : suffix가 접미사인데?
* 남규 : 아 맞네.. 접두사로 착각했네.
* 택희 : 근데 그럼 접두사 배열은 어떻게 구하지?
* 남규 : 그러게?
* 택희 : 문자열 뒤집고 suffix array 구하면 되나? 아닌데..?\
* 택희와 남규는 혼란에 빠졌다.

혼란스러워하는 택희와 남규를 위해 접두사 배열을 구해 줄 프로그램을 작성해 보자.

출처 : https://www.acmicpc.net/problem/13322

---

#### 올바른 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String word = scanner.nextLine();
        for (int i = 0; i < word.length(); i++) {
            System.out.println(i);
        }
    }
}
~~~

#### 연습용 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Prefixes prefixes = Prefixes.of(scanner.nextLine());
        prefixes.getSortedPrefixesIndexes().forEach(System.out::println);
    }
}

class Prefixes {
    private final List<Prefix> prefixes;

    private Prefixes(List<Prefix> prefixes) {
        this.prefixes = prefixes;
    }

    public static Prefixes of(String word) {
        return new Prefixes(convertPrefixes(word));
    }

    public List<Integer> getSortedPrefixesIndexes() {
        return this.prefixes.stream()
                .sorted()
                .map(Prefix::getIndex)
                .collect(Collectors.toList());
    }

    private static List<Prefix> convertPrefixes(String word) {
        return IntStream.range(0, word.length())
                .mapToObj(i -> new Prefix(word.substring(0, i + 1), i))
                .collect(Collectors.toList());
    }
}

class Prefix implements Comparable<Prefix> {
    private final String prefix;
    private final int index;

    public Prefix(String prefix, int index) {
        this.prefix = prefix;
        this.index = index;
    }

    public int getIndex() {
        return index;
    }

    @Override
    public int compareTo(Prefix o) {
        return this.prefix.compareTo(o.prefix);
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
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideWords")
    @DisplayName("접두사 배열의 정렬 인덱스를 반환한다.")
    void prefixArraySortTest(String word, List<Integer> expected) {
        Prefixes sut = Prefixes.of(word);

        List<Integer> actual = sut.getSortedPrefixesIndexes();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideWords() {
        return Stream.of(
                Arguments.of("ab", List.of(0, 1)),
                Arguments.of("banana", List.of(0, 1, 2, 3, 4, 5))
        );
    }

    @ParameterizedTest
    @MethodSource("provideWordsAndPrefixArray")
    @DisplayName("접두사 목록을 반환한다.")
    void prefixArrayTest(String word, List<String> expected) {
        List<String> actual = getPrefixes(word);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideWordsAndPrefixArray() {
        return Stream.of(
                Arguments.of("ab", List.of("a", "ab")),
                Arguments.of("abc", List.of("a", "ab", "abc")),
                Arguments.of("good", List.of("g", "go", "goo", "good")),
                Arguments.of("banana", List.of("b", "ba", "ban", "bana", "banan", "banana")),
                Arguments.of("a", List.of("a"))
        );
    }

    private List<String> getPrefixes(String word) {
        return IntStream.range(0, word.length())
                .mapToObj(i -> word.substring(0, i + 1))
                .collect(Collectors.toList());
    }
}

class Prefixes {
    private final List<Prefix> prefixes;

    private Prefixes(List<Prefix> prefixes) {
        this.prefixes = prefixes;
    }

    public static Prefixes of(String word) {
        return new Prefixes(convertPrefixes(word));
    }

    public List<Integer> getSortedPrefixesIndexes() {
        return this.prefixes.stream()
                .sorted()
                .map(Prefix::getIndex)
                .collect(Collectors.toList());
    }

    private static List<Prefix> convertPrefixes(String word) {
        return IntStream.range(0, word.length())
                .mapToObj(i -> new Prefix(word.substring(0, i + 1), i))
                .collect(Collectors.toList());
    }
}

class Prefix implements Comparable<Prefix> {
    private final String prefix;
    private final int index;

    public Prefix(String prefix, int index) {
        this.prefix = prefix;
        this.index = index;
    }

    public int getIndex() {
        return index;
    }

    @Override
    public int compareTo(Prefix o) {
        return this.prefix.compareTo(o.prefix);
    }
}
~~~
