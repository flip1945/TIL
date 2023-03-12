# 문자열 밀기 (Level 0)

### 문제 설명

문자열 "hello"에서 각 문자를 오른쪽으로 한 칸씩 밀고 마지막 문자는 맨 앞으로 이동시키면 "ohell"이 됩니다. 이것을 문자열을 민다고 정의한다면 문자열 A와 B가 매개변수로 주어질 때, A를 밀어서 B가 될 수 있다면 몇 번 밀어야 하는지 횟수를 return하고 밀어서 B가 될 수 없으면 -1을 return 하도록 solution 함수를 완성해보세요.

#### 제한사항

* 0 < A의 길이 = B의 길이 < 100
* A, B는 알파벳 소문자로 이루어져 있습니다.

출처 : https://programmers.co.kr/learn/courses/30/lessons/120921

---

#### 풀이

~~~java
import java.util.LinkedList;
import java.util.List;

public class Solution {
    public int solution(String word, String targetWord) {
        for (int i = 0; i < word.length(); i++) {
            if (word.equals(targetWord)) {
                return i;
            }
            word = rotate(word);
        }
        return -1;
    }

    private String rotate(String word) {
        List<String> strings = new LinkedList<>(List.of(word.split("")));
        strings.add(0, strings.remove(strings.size()-1));
        return String.join("", strings);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.LinkedList;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void stringRotateTest() {
        String word1 = "hello";
        String word2 = "ohell";
        String word3 = "lohel";
        String word4 = "llohe";
        String word5 = "a";

        assertEquals("ohell", rotate(word1));
        assertEquals("lohel", rotate(word2));
        assertEquals("llohe", rotate(word3));
        assertEquals("elloh", rotate(word4));
        assertEquals("a", rotate(word5));
    }

    @Test
    void solutionTest() {
        String word1 = "hello";
        String word2 = "apple";
        String word3 = "a";
        String word4 = "hello";
        String targetWord1 = "ohell";
        String targetWord2 = "elppa";
        String targetWord3 = "b";
        String targetWord4 = "a";
        String targetWord5 = "elloh";

        assertEquals(1, solution(word1, targetWord1));
        assertEquals(-1, solution(word2, targetWord2));
        assertEquals(-1, solution(word3, targetWord3));
        assertEquals(0, solution(word3, targetWord4));
        assertEquals(4, solution(word4, targetWord5));
    }

    private int solution(String word, String targetWord) {
        for (int i = 0; i < word.length(); i++) {
            if (word.equals(targetWord)) {
                return i;
            }
            word = rotate(word);
        }
        return -1;
    }

    private String rotate(String word) {
        List<String> strings = new LinkedList<>(List.of(word.split("")));
        strings.add(0, strings.remove(strings.size()-1));
        return String.join("", strings);
    }
}
~~~
