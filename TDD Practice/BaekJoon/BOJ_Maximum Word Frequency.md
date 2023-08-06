# Maximum Word Frequency (Silver 4)

### 문제 설명

Term frequency–Inverse document frequency (tf-idf) is a numerical statistic which reflects the importance of words in a document collection. It is often used in information retrieval system. The number of times a word appears in the document (word frequency) is one of the major factors to acquire tf-idf. 

You are asked to write a program to find the most frequent word in a document. 

출처 : https://www.acmicpc.net/problem/9612

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<String> words = inputWords(scanner, n);

        WordCounter counter = new WordCounter(words);
        System.out.println(counter.getMaxFrequentWordAndCount());
    }

    private static List<String> inputWords(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class WordCounter {

    private final Map<String, Integer> wordCounter = new TreeMap<>(Comparator.reverseOrder());
    private final List<String> words;

    public WordCounter(List<String> words) {
        this.words = words;
    }

    public String getMaxFrequentWordAndCount() {
        countWords();
        return getMaxFrequentWord() + " " + wordCounter.get(getMaxFrequentWord());
    }

    private void countWords() {
        words.forEach(word -> wordCounter.merge(word, 1, Integer::sum));
    }

    private String getMaxFrequentWord() {
        return wordCounter.entrySet()
                .stream()
                .sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))
                .map(Map.Entry::getKey)
                .findFirst()
                .orElseThrow();
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
    @DisplayName("주어진 단어 중 가장 많이 나타난 단어와 횟수를 반환한다.")
    void testGetMaxFrequentWordAndCount(List<String> words, String expected) {
        WordCounter sut = new WordCounter(words);

        String actual = sut.getMaxFrequentWordAndCount();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideWords() {
        return Stream.of(
                Arguments.of(List.of("a"), "a 1"),
                Arguments.of(List.of("b"), "b 1"),
                Arguments.of(List.of("a", "b", "a"), "a 2"),
                Arguments.of(List.of("a", "b", "c", "c"), "c 2"),
                Arguments.of(List.of("a", "a", "b", "c", "c"), "c 2"),
                Arguments.of(List.of("c", "c", "b", "a", "a"), "c 2"),
                Arguments.of(List.of("mountain", "lake", "lake", "zebra", "tree", "lake", "zebra", "zebra", "animal",
                        "lakes"), "zebra 3")
        );
    }
}

class WordCounter {

    private final Map<String, Integer> wordCounter = new TreeMap<>(Comparator.reverseOrder());
    private final List<String> words;

    public WordCounter(List<String> words) {
        this.words = words;
    }

    public String getMaxFrequentWordAndCount() {
        countWords();
        return getMaxFrequentWord() + " " + wordCounter.get(getMaxFrequentWord());
    }

    private void countWords() {
        words.forEach(word -> wordCounter.merge(word, 1, Integer::sum));
    }

    private String getMaxFrequentWord() {
        return wordCounter.entrySet()
                .stream()
                .sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))
                .map(Map.Entry::getKey)
                .findFirst()
                .orElseThrow();
    }
}
~~~
