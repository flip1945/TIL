# 밑 줄 (Silver 1)

### 문제 설명

세준이는 N개의 영어 단어를 이용해 길이가 M인 새로운 단어를 만들려고 한다. 새로운 단어는 N개의 단어를 순서대로 이어 붙이고, 각 단어의 사이에 \_을 넣어서 만든다. 이렇게 만든 새로운 단어의 길이가 M이 아닌 경우 \_를 추가해서 길이가 M이 되게 만들어야 한다.

* \_는 단어와 단어 사이에만 추가할 수 있다. 따라서, 새로운 단어는 \_으로 시작하거나, \_로 끝날 수 없다.
* 단어와 단어 사이에 있는 \_의 개수는 모두 같아야 한다.
  * 모두 같게 만드는 것이 불가능한 경우 단어와 단어 사이에 있는 \_의 개수의 최댓값과 최솟값의 차이는 1이 되어야 한다.

새로운 단어 중 사전 순으로 가장 앞서는 단어를 구해보자. 

출처 : https://www.acmicpc.net/problem/1474

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        List<String> words = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            words.add(scanner.nextLine());
        }

        System.out.println(createNewWordOfUnderBar(words, m));
    }

    static String createNewWordOfUnderBar(List<String> words, int newWordSize) {
        StringBuilder newWord = new StringBuilder(words.get(0));
        int countOfUnderBars = words.size() - 1;
        int averageUnderBarSize = getAverageUnderBarSize(newWordSize - sunWordLengths(words), countOfUnderBars);
        int plusUnderBarCount = getPlusUnderBarCount(newWordSize - sunWordLengths(words), countOfUnderBars, averageUnderBarSize);
        for (int i = 1; i < words.size(); i++) {
            plusUnderBarCount = getPlusUnderBarCountAndAppendNewWord(words, newWord, averageUnderBarSize, plusUnderBarCount, i);
        }
        return newWord.toString();
    }

    static int getAverageUnderBarSize(int wordSize, int countOfUnderBar) {
        return wordSize / countOfUnderBar;
    }

    static int getPlusUnderBarCount(int wordSize, int countOfUnderBar, int averageUnderBarSize) {
        return wordSize - (averageUnderBarSize * countOfUnderBar);
    }

    static int sunWordLengths(List<String> words) {
        return words.stream()
                .mapToInt(String::length)
                .sum();
    }

    static int getPlusUnderBarCountAndAppendNewWord(List<String> words, StringBuilder newWord, int averageUnderBarSize, int plusUnderBarCount, int i) {
        String word = words.get(i);
        if (plusUnderBarCount > 0 && (Character.isLowerCase(word.charAt(0)) || (Character.isUpperCase(word.charAt(0)) && words.size() - i <= plusUnderBarCount))) {
            newWord.append("_".repeat(averageUnderBarSize + 1)).append(word);
            return plusUnderBarCount - 1;
        }
        newWord.append("_".repeat(averageUnderBarSize)).append(word);
        return plusUnderBarCount;
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
    void createNewWordOfUnderBarTest() {
        assertEquals("a_b_c", createNewWordOfUnderBar(List.of("a", "b", "c"), 5));
        assertEquals("a__b_c", createNewWordOfUnderBar(List.of("a", "b", "c"), 6));
        assertEquals("a__b__c", createNewWordOfUnderBar(List.of("a", "b", "c"), 7));
        assertEquals("a___b__c", createNewWordOfUnderBar(List.of("a", "b", "c"), 8));
        assertEquals("A___quick__brown__fox__jumps__over__the__lazy__dog", createNewWordOfUnderBar(List.of("A", "quick", "brown", "fox", "jumps", "over", "the", "lazy", "dog"), 50));
        assertEquals("Alpha_Beta_Gamma__Delta__Epsilon", createNewWordOfUnderBar(List.of("Alpha", "Beta", "Gamma", "Delta", "Epsilon"), 32));
        assertEquals("Hello____world___John____said", createNewWordOfUnderBar(List.of("Hello", "world", "John", "said"), 29));
    }

    @Test
    void getWordLengthsTest() {
        assertEquals(6, sunWordLengths(List.of("a", "is", "top")));
        assertEquals(3, sunWordLengths(List.of("a", "is")));
        assertEquals(1, sunWordLengths(List.of("a")));
        assertEquals(33, sunWordLengths(List.of("A", "quick", "brown", "fox", "jumps", "over", "the", "lazy", "dog")));
    }

    private String createNewWordOfUnderBar(List<String> words, int newWordSize) {
        StringBuilder newWord = new StringBuilder(words.get(0));
        int countOfUnderBars = words.size() - 1;
        int averageUnderBarSize = getAverageUnderBarSize(newWordSize - sunWordLengths(words), countOfUnderBars);
        int plusUnderBarCount = getPlusUnderBarCount(newWordSize - sunWordLengths(words), countOfUnderBars, averageUnderBarSize);
        for (int i = 1; i < words.size(); i++) {
            plusUnderBarCount = getPlusUnderBarCountAndAppendNewWord(words, newWord, averageUnderBarSize, plusUnderBarCount, i);
        }
        return newWord.toString();
    }

    private int getAverageUnderBarSize(int wordSize, int countOfUnderBar) {
        return wordSize / countOfUnderBar;
    }

    private int getPlusUnderBarCount(int wordSize, int countOfUnderBar, int averageUnderBarSize) {
        return wordSize - (averageUnderBarSize * countOfUnderBar);
    }

    private int sunWordLengths(List<String> words) {
        return words.stream()
                .mapToInt(String::length)
                .sum();
    }

    private int getPlusUnderBarCountAndAppendNewWord(List<String> words, StringBuilder newWord, int averageUnderBarSize, int plusUnderBarCount, int i) {
        String word = words.get(i);
        if (plusUnderBarCount > 0 && (Character.isLowerCase(word.charAt(0)) || (Character.isUpperCase(word.charAt(0)) && words.size() - i <= plusUnderBarCount))) {
            newWord.append("_".repeat(averageUnderBarSize + 1)).append(word);
            return plusUnderBarCount - 1;
        }
        newWord.append("_".repeat(averageUnderBarSize)).append(word);
        return plusUnderBarCount;
    }
}
~~~
