# 잘라서 배열로 저장하기 (Level 0)

### 문제 설명

문자열 my_str과 n이 매개변수로 주어질 때, my_str을 길이 n씩 잘라서 저장한 배열을 return하도록 solution 함수를 완성해주세요.

#### 제한사항

* 1 ≤ my_str의 길이 ≤ 100
* 1 ≤ n ≤ my_str의 길이
* my_str은 알파벳 소문자, 대문자, 숫자로 이루어져 있습니다.

출처 : https://programmers.co.kr/learn/courses/30/lessons/120913

---

#### 풀이

~~~java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public String[] solution(String word, int n) {
        return toArray(splitWord(word, n));
    }

    private String[] toArray(List<String> words) {
        String[] answer = new String[words.size()];
        for (int i = 0; i < words.size(); i++) {
            answer[i] = words.get(i);
        }
        return answer;
    }

    private List<String> splitWord(String word, int n) {
        List<String> words = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < word.length(); i++) {
            sb = getStringBuilder(word, n, words, sb, i);
        }
        return words;
    }

    private StringBuilder getStringBuilder(String word, int n, List<String> words, StringBuilder sb, int i) {
        sb.append(word.charAt(i));
        if (canSplit(sb, n, i, word.length())) {
            words.add(sb.toString());
            sb = new StringBuilder();
        }
        return sb;
    }

    private boolean canSplit(StringBuilder sb, int n, int i, int totalSize) {
        return sb.length() == n || i + 1 == totalSize;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertArrayEquals;

class MainTest {

    @Test
    void solutionTest() {
        String word1 = "abc";
        String word2 = "abcdef";
        String word3 = "abcdef123";
        String word4 = "abc1Addfggg4556b";
        int n1 = 1;
        int n2 = 3;
        int n3 = 4;
        int n4 = 6;

        assertArrayEquals(new String[]{"a", "b", "c"}, solution(word1, n1));
        assertArrayEquals(new String[]{"a", "b", "c", "d", "e", "f"}, solution(word2, n1));
        assertArrayEquals(new String[]{"abc", "def"}, solution(word2, n2));
        assertArrayEquals(new String[]{"abcd", "ef"}, solution(word2, n3));
        assertArrayEquals(new String[]{"abc", "def", "123"}, solution(word3, n2));
        assertArrayEquals(new String[]{"abc1Ad", "dfggg4", "556b"}, solution(word4, n4));
    }

    public String[] solution(String word, int n) {
        return toArray(splitWord(word, n));
    }

    private String[] toArray(List<String> words) {
        String[] answer = new String[words.size()];
        for (int i = 0; i < words.size(); i++) {
            answer[i] = words.get(i);
        }
        return answer;
    }

    private List<String> splitWord(String word, int n) {
        List<String> words = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < word.length(); i++) {
            sb = getStringBuilder(word, n, words, sb, i);
        }
        return words;
    }

    private StringBuilder getStringBuilder(String word, int n, List<String> words, StringBuilder sb, int i) {
        sb.append(word.charAt(i));
        if (canSplit(sb, n, i, word.length())) {
            words.add(sb.toString());
            sb = new StringBuilder();
        }
        return sb;
    }

    private boolean canSplit(StringBuilder sb, int n, int i, int totalSize) {
        return sb.length() == n || i + 1 == totalSize;
    }
}
~~~
