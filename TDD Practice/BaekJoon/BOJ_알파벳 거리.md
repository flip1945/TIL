# 알파벳 거리 (Bronze 2)

### 문제 설명

길이가 같은 두 단어가 주어졌을 때, 각 단어에 포함된 모든 글자의 알파벳 거리를 구하는 프로그램을 작성하시오.

두 글자 x와 y 사이의 알파벳 거리를 구하려면, 먼저 각 알파벳에 숫자를 할당해야 한다. 'A'=1, 'B' = 2, ..., 'Z' = 26. 그 다음 y ≥ x인 경우에는 y-x, y < x인 경우에는 (y+26) - x가 알파벳 거리가 된다.

예를 들어, 'B'와 'D' 사이의 거리는 4 - 2 = 2이고, 'D'와 'B' 사이의 거리는 (2+26) - 4 = 24이다.

출처 : https://www.acmicpc.net/problem/5218

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            StringTokenizer st = new StringTokenizer(scanner.nextLine());
            WordDistance wordDistance = new WordDistance(st.nextToken(), st.nextToken());
            System.out.println(wordDistance);
        }
    }
}

class WordDistance {

    private static final String DISTANCE_PREFIX = "Distances: ";
    private static final String DELIMITER = " ";

    private final String firstWord;
    private final String secondWord;

    public WordDistance(String firstWord, String secondWord) {
        this.firstWord = firstWord;
        this.secondWord = secondWord;
    }

    @Override
    public String toString() {
        return DISTANCE_PREFIX + getDistancesString();
    }

    private String getDistancesString() {
        List<Integer> distances = getDistancesBetweenTwoWords();
        return distances.stream()
                .map(String::valueOf)
                .collect(Collectors.joining(DELIMITER));
    }

    private List<Integer> getDistancesBetweenTwoWords() {
        return IntStream.range(0, firstWord.length())
                .mapToObj(i -> new AlphabetDistance(firstWord.charAt(i), secondWord.charAt(i)))
                .map(AlphabetDistance::getDistance)
                .collect(Collectors.toList());
    }
}

class AlphabetDistance {

    private final char firstAlphabet;
    private final char secondAlphabet;

    public AlphabetDistance(char firstAlphabet, char secondAlphabet) {
        this.firstAlphabet = firstAlphabet;
        this.secondAlphabet = secondAlphabet;
    }

    public int getDistance() {
        if (firstAlphabet > secondAlphabet) {
            return secondAlphabet - firstAlphabet + 26;
        }
        return secondAlphabet - firstAlphabet;
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

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideTwoAlphabet")
    @DisplayName("alphabetDistance는 두 알파벳이 주어지면 두 문자간 거리를 계산한다.")
    void alphabetDistanceReturnsDistanceBetweenTwoAlphabet(char firstAlphabet, char secondAlphabet, int expected) {
        AlphabetDistance sut = new AlphabetDistance(firstAlphabet, secondAlphabet);

        int actual = sut.getDistance();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideTwoAlphabet() {
        return Stream.of(
                Arguments.of('A', 'B', 1),
                Arguments.of('A', 'C', 2),
                Arguments.of('B', 'D', 2),
                Arguments.of('D', 'B', 24)
        );
    }

    @ParameterizedTest
    @MethodSource("provideTwoWords")
    @DisplayName("wordDistance는 두 단어가 주어지면 두 단어의 거리를 계산한다.")
    void wordDistanceReturnsDistanceBetweenTwoWords(String firstWord, String secondWord, String expected) {
        WordDistance sut = new WordDistance(firstWord, secondWord);

        String actual = sut.toString();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideTwoWords() {
        return Stream.of(
                Arguments.of("AAAA", "ABCD", "Distances: 0 1 2 3"),
                Arguments.of("ABCD", "AAAA", "Distances: 0 25 24 23"),
                Arguments.of("DARK", "LOKI", "Distances: 8 14 19 24"),
                Arguments.of("STRONG", "THANOS", "Distances: 1 14 9 25 1 12"),
                Arguments.of("DEADLY", "ULTIMO", "Distances: 17 7 19 5 1 16")
        );
    }
}

class WordDistance {

    private static final String DISTANCE_PREFIX = "Distances: ";
    private static final String DELIMITER = " ";

    private final String firstWord;
    private final String secondWord;

    public WordDistance(String firstWord, String secondWord) {
        this.firstWord = firstWord;
        this.secondWord = secondWord;
    }

    @Override
    public String toString() {
        return DISTANCE_PREFIX + getDistancesString();
    }

    private String getDistancesString() {
        List<Integer> distances = getDistancesBetweenTwoWords();
        return distances.stream()
                .map(String::valueOf)
                .collect(Collectors.joining(DELIMITER));
    }

    private List<Integer> getDistancesBetweenTwoWords() {
        return IntStream.range(0, firstWord.length())
                .mapToObj(i -> new AlphabetDistance(firstWord.charAt(i), secondWord.charAt(i)))
                .map(AlphabetDistance::getDistance)
                .collect(Collectors.toList());
    }
}

class AlphabetDistance {

    private final char firstAlphabet;
    private final char secondAlphabet;

    public AlphabetDistance(char firstAlphabet, char secondAlphabet) {
        this.firstAlphabet = firstAlphabet;
        this.secondAlphabet = secondAlphabet;
    }

    public int getDistance() {
        if (firstAlphabet > secondAlphabet) {
            return secondAlphabet - firstAlphabet + 26;
        }
        return secondAlphabet - firstAlphabet;
    }
}
~~~
