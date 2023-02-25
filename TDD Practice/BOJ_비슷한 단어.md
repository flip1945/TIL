# 비슷한 단어 (Silver 2)

### 문제 설명

만약 어떤 단어A를 숌스럽게 바꿔서 또다른 단어 B로 만든다면, 그 단어는 비슷한 단어라고 한다.

어떤 단어를 숌스럽게 바꾼다는 말은 단어 A에 등장하는 모든 알파벳을 다른 알파벳으로 바꾼다는 소리다. 그리고, 단어에 등장하는 알파벳의 순서는 바뀌지 않는다. 두 개의 다른 알파벳을 하나의 알파벳으로 바꿀 수 없지만, 자기 자신으로 바꾸는 것은 가능하다.

예를 들어, 단어 abca와 zbxz는 비슷하다. 그 이유는 a를 z로 바꾸고, b는 그대로 b, c를 x로 바꾸면, abca가 zbxz가된다.

단어가 여러 개 주어졌을 때, 몇 개의 쌍이 비슷한지 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1411

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        String[] words = new String[n];
        for (int i = 0; i < n; i++) {
            words[i] = scanner.nextLine();
        }
        System.out.println(getCountOfSimilarWords(n, words));
    }

    static int getCountOfSimilarWords(int n, String[] words) {
        int countOfSimilarWords = 0;
        for (int i = 0; i < n; i++) {
            countOfSimilarWords += getCountOfSimilarWordsOfOneWord(words[i], Arrays.copyOfRange(words, i + 1, n));
        }
        return countOfSimilarWords;
    }

    static int getCountOfSimilarWordsOfOneWord(String basicWord, String[] words) {
        int countOfSimilarWordsOfOneWord = 0;
        for (String word : words) {
            if (isSimilarWord(basicWord, word)) {
                countOfSimilarWordsOfOneWord++;
            }
        }
        return countOfSimilarWordsOfOneWord;
    }

    static boolean isSimilarWord(String word1, String word2) {
        return toWordNumbers(word1).equals(toWordNumbers(word2));
    }

    static List<Integer> toWordNumbers(String word) {
        List<Integer> wordNumbers = new ArrayList<>();
        Map<Character, Integer> wordNumberDictionary = new HashMap<>();
        int wordNumber = 0;
        for (char currentCharacter : word.toCharArray()) {
            int currentCharacterNumber = wordNumberDictionary.getOrDefault(currentCharacter, wordNumber++);
            wordNumberDictionary.put(currentCharacter, currentCharacterNumber);
            wordNumbers.add(currentCharacterNumber);
        }
        return wordNumbers;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.*;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void isSimilarWordTest() {
        assertEquals(true, isSimilarWord("aa", "bb"));
        assertEquals(false, isSimilarWord("aa", "ac"));
        assertEquals(false, isSimilarWord("aa", "bc"));
        assertEquals(true, isSimilarWord("ab", "cd"));
        assertEquals(false, isSimilarWord("cc", "ab"));
        assertEquals(true, isSimilarWord("abca", "zbxz"));
        assertEquals(false, isSimilarWord("abca", "opqr"));
        assertEquals(false, isSimilarWord("zbxz", "opqr"));
        assertEquals(true, isSimilarWord("abcd", "asdf"));
        assertEquals(true, isSimilarWord("abcd", "abcf"));
        assertEquals(false, isSimilarWord("ab", "cc"));
        assertEquals(false, isSimilarWord("aabba", "ccddd"));
    }

    @Test
    void getCountOfSimilarWordsTest() {
        assertEquals(4, getCountOfSimilarWords(5, new String[]{"aa", "ab", "bb", "cc", "cd"}));
        assertEquals(1, getCountOfSimilarWords(3, new String[]{"abca", "zbxz", "opqr"}));
        assertEquals(13, getCountOfSimilarWords(12, new String[]{"cacccdaabc", "cdcccaddbc", "dcdddbccad", "bdbbbaddcb", "bdbcadbbdc", "abaadcbbda", "babcdabbac", "cacdbaccad", "dcddabccad", "cacccbaadb", "bbcdcbcbdd", "bcbadcbbca"}));
        assertEquals(6, getCountOfSimilarWords(4, new String[]{"aa", "bb", "cc", "dd"}));
        assertEquals(0, getCountOfSimilarWords(2, new String[]{"aa", "cb"}));
        assertEquals(0, getCountOfSimilarWords(1, new String[]{"aa"}));
    }

    private int getCountOfSimilarWords(int n, String[] words) {
        int countOfSimilarWords = 0;
        for (int i = 0; i < n; i++) {
            countOfSimilarWords += getCountOfSimilarWordsOfOneWord(words[i], Arrays.copyOfRange(words, i + 1, n));
        }
        return countOfSimilarWords;
    }

    private int getCountOfSimilarWordsOfOneWord(String basicWord, String[] words) {
        int countOfSimilarWordsOfOneWord = 0;
        for (String word : words) {
            if (isSimilarWord(basicWord, word)) {
                countOfSimilarWordsOfOneWord++;
            }
        }
        return countOfSimilarWordsOfOneWord;
    }

    private boolean isSimilarWord(String word1, String word2) {
        return toWordNumbers(word1).equals(toWordNumbers(word2));
    }

    private List<Integer> toWordNumbers(String word) {
        List<Integer> wordNumbers = new ArrayList<>();
        Map<Character, Integer> wordNumberDictionary = new HashMap<>();
        int wordNumber = 0;
        for (char currentCharacter : word.toCharArray()) {
            int currentCharacterNumber = wordNumberDictionary.getOrDefault(currentCharacter, wordNumber++);
            wordNumberDictionary.put(currentCharacter, currentCharacterNumber);
            wordNumbers.add(currentCharacterNumber);
        }
        return wordNumbers;
    }
}
~~~
