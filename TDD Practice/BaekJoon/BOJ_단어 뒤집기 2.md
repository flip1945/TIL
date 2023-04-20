# 단어 뒤집기 2 (Silver 3)

### 문제 설명

문자열 S가 주어졌을 때, 이 문자열에서 단어만 뒤집으려고 한다.

먼저, 문자열 S는 아래와과 같은 규칙을 지킨다.

1. 알파벳 소문자('a'-'z'), 숫자('0'-'9'), 공백(' '), 특수 문자('<', '>')로만 이루어져 있다.
2. 문자열의 시작과 끝은 공백이 아니다.
3. '<'와 '>'가 문자열에 있는 경우 번갈아가면서 등장하며, '<'이 먼저 등장한다. 또, 두 문자의 개수는 같다.

태그는 '<'로 시작해서 '>'로 끝나는 길이가 3 이상인 부분 문자열이고, '<'와 '>' 사이에는 알파벳 소문자와 공백만 있다. 단어는 알파벳 소문자와 숫자로 이루어진 부분 문자열이고, 연속하는 두 단어는 공백 하나로 구분한다. 태그는 단어가 아니며, 태그와 단어 사이에는 공백이 없다.

출처 : https://www.acmicpc.net/problem/17413

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(reverseOnlyWord(scanner.nextLine()));
    }

    static String reverseOnlyWord(String word) {
        StringBuilder result = new StringBuilder();
        List<String> splitWord = splitWord(word);
        result.append(reverseIfNotTag(splitWord.get(0)));

        for (int i = 1; i < splitWord.size(); i++) {
            if (isNotTag(splitWord.get(i - 1)) && isNotTag(splitWord.get(i))) {
                result.append(" ");
            }
            result.append(reverseIfNotTag(splitWord.get(i)));
        }
        return result.toString();
    }

    static List<String> splitWord(String word) {
        String[] splitWord = getSplitWordByThanSign(word);
        return getSplitWords(splitWord);
    }

    static String[] getSplitWordByThanSign(String word) {
        return word.replaceAll(">", "> ")
                .replaceAll("<", " <")
                .replaceAll(">[ ]+<", "> <")
                .trim()
                .split(" ");
    }

    static List<String> getSplitWords(String[] splitWord) {
        List<String> result = new ArrayList<>();
        for (int i = 0; i < splitWord.length; i++) {
            if (splitWord[i].startsWith("<") && !splitWord[i].endsWith(">")) {
                StringBuilder sb = new StringBuilder();
                while (!splitWord[i].endsWith(">")) {
                    sb.append(splitWord[i++]).append(" ");
                }
                sb.append(splitWord[i]);
                result.add(sb.toString());
            } else {
                result.add(splitWord[i]);
            }
        }
        return result;
    }

    static String reverseIfNotTag(String word) {
        if (isNotTag(word)) {
            return new StringBuilder(word).reverse().toString();
        }
        return word;
    }

    static boolean isNotTag(String word) {
        return !word.startsWith("<");
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
    void splitWordTest() {
        assertEquals(List.of("a", "b", "c"), splitWord("a b c"));
        assertEquals(List.of("a", "a", "a"), splitWord("a a a"));
        assertEquals(List.of("baek", "joon"), splitWord("baek joon"));
        assertEquals(List.of("<tag>", "baek", "joon"), splitWord("<tag>baek joon"));
        assertEquals(List.of("<open>", "tag", "<close>"), splitWord("<open>tag<close>"));
        assertEquals(List.of("<int>", "<max>", "1", "<long long>", "<max>", "2"), splitWord("<int><max>1<long long><max>2"));
        assertEquals(List.of("<  int  >", "s", "s", "s", "< in t   >"), splitWord("<  int  >s s s< in t   >"));
    }

    @Test
    void reverseOnlyWordTest() {
        assertEquals("noojkeab enilno egduj", reverseOnlyWord("baekjoon online judge"));
        assertEquals("<open>gat<close>", reverseOnlyWord("<open>tag<close>"));
        assertEquals("<ab cd>fe hg<ij kl>", reverseOnlyWord("<ab cd>ef gh<ij kl>"));
        assertEquals("1eno 2owt 3eerht rruof4 evif5 xis6", reverseOnlyWord("one1 two2 three3 4fourr 5five 6six"));
        assertEquals("<int><max>7463847412<long long><max>7085774586302733229", reverseOnlyWord("<int><max>2147483647<long long><max>9223372036854775807"));
        assertEquals("<problem>31471<is hardest>melborp reve<end>", reverseOnlyWord("<problem>17413<is hardest>problem ever<end>"));
        assertEquals("<   space   >ecaps ecaps ecaps<    spa   c e>", reverseOnlyWord("<   space   >space space space<    spa   c e>"));
    }

    private String reverseOnlyWord(String word) {
        StringBuilder result = new StringBuilder();
        List<String> splitWord = splitWord(word);
        result.append(reverseIfNotTag(splitWord.get(0)));

        for (int i = 1; i < splitWord.size(); i++) {
            if (isNotTag(splitWord.get(i - 1)) && isNotTag(splitWord.get(i))) {
                result.append(" ");
            }
            result.append(reverseIfNotTag(splitWord.get(i)));
        }
        return result.toString();
    }

    private List<String> splitWord(String word) {
        String[] splitWord = getSplitWordByThanSign(word);
        return getSplitWords(splitWord);
    }

    private String[] getSplitWordByThanSign(String word) {
        return word.replaceAll(">", "> ")
                .replaceAll("<", " <")
                .replaceAll(">[ ]+<", "> <")
                .trim()
                .split(" ");
    }

    private List<String> getSplitWords(String[] splitWord) {
        List<String> result = new ArrayList<>();
        for (int i = 0; i < splitWord.length; i++) {
            if (splitWord[i].startsWith("<") && !splitWord[i].endsWith(">")) {
                StringBuilder sb = new StringBuilder();
                while (!splitWord[i].endsWith(">")) {
                    sb.append(splitWord[i++]).append(" ");
                }
                sb.append(splitWord[i]);
                result.add(sb.toString());
            } else {
                result.add(splitWord[i]);
            }
        }
        return result;
    }

    private String reverseIfNotTag(String word) {
        if (isNotTag(word)) {
            return new StringBuilder(word).reverse().toString();
        }
        return word;
    }

    private boolean isNotTag(String word) {
        return !word.startsWith("<");
    }
}
~~~
