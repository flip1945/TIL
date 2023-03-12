# 외계어 사전 (Level 0)

### 문제 설명

PROGRAMMERS-962 행성에 불시착한 우주비행사 머쓱이는 외계행성의 언어를 공부하려고 합니다. 알파벳이 담긴 배열 spell과 외계어 사전 dic이 매개변수로 주어집니다. spell에 담긴 알파벳을 한번씩만 모두 사용한 단어가 dic에 존재한다면 1, 존재하지 않는다면 2를 return하도록 solution 함수를 완성해주세요.

#### 제한사항

* spell과 dic의 원소는 알파벳 소문자로만 이루어져있습니다.
* 2 ≤ spell의 크기 ≤ 10
* spell의 원소의 길이는 1입니다.
* 1 ≤ dic의 크기 ≤ 10
* 1 ≤ dic의 원소의 길이 ≤ 10
* spell의 원소를 모두 사용해 단어를 만들어야 합니다.
* spell의 원소를 모두 사용해 만들 수 있는 단어는 dic에 두 개 이상 존재하지 않습니다.
* dic과 spell 모두 중복된 원소를 갖지 않습니다.

출처 : https://programmers.co.kr/learn/courses/30/lessons/120869

---

#### 풀이

~~~java
class Solution {
    public int solution(String[] spell, String[] dic) {
        for (String word : dic) {
            if (isAlienLanguage(spell, word)) {
                return 1;
            }
        }
        return 2;
    }

    private boolean isAlienLanguage(String[] spell, String word) {
        for (String s : spell) {
            if (!isOnlyOneContain(s, word)) {
                return false;
            }
        }
        return true;
    }

    private boolean isOnlyOneContain(String spell, String word) {
        int count = 0;
        for (char character : word.toCharArray()) {
            if (spell.charAt(0) == character) {
                count++;
            }
        }
        return count == 1;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void isOnlyOneContainTes() {
        String spell = "p";
        String word1 = "pod";
        String word2 = "ooo";
        String word3 = "ppad";
        String word4 = "ppppp";

        assertEquals(true, isOnlyOneContain(spell, word1));
        assertEquals(false, isOnlyOneContain(spell, word2));
        assertEquals(false, isOnlyOneContain(spell, word3));
        assertEquals(false, isOnlyOneContain(spell, word4));
    }

    @Test
    void isAlienLanguageTest() {
        String[] spell1 = {"p", "o", "s"};
        String[] spell2 = {"z", "d", "x"};
        String word1 = "sod";
        String word2 = "pods";
        String word3 = "ppas";
        String word4 = "def";
        String word5 = "dww";
        String word6 = "dzx";
        String word7 = "loveaw";

        assertEquals(false, isAlienLanguage(spell1, word1));
        assertEquals(true, isAlienLanguage(spell1, word2));
        assertEquals(false, isAlienLanguage(spell1, word3));
        assertEquals(false, isAlienLanguage(spell2, word4));
        assertEquals(false, isAlienLanguage(spell2, word5));
        assertEquals(true, isAlienLanguage(spell2, word6));
        assertEquals(false, isAlienLanguage(spell2, word7));
    }

    @Test
    void solutionTest() {
        String[] spell1 = {"p", "o", "s"};
        String[] spell2 = {"z", "d", "x"};
        String[] spell3 = {"s", "o", "m", "d"};
        String[] dic1 = {"sod", "eocd", "qixm", "adio", "soo"};
        String[] dic2 = {"def", "dww", "dzx", "loveaw"};
        String[] dic3 = {"moos", "dzx", "smm", "sunmmo", "som"};

        assertEquals(2, solution(spell1, dic1));
        assertEquals(1, solution(spell2, dic2));
        assertEquals(2, solution(spell3, dic3));
    }

    public int solution(String[] spell, String[] dic) {
        for (String word : dic) {
            if (isAlienLanguage(spell, word)) {
                return 1;
            }
        }
        return 2;
    }

    private boolean isAlienLanguage(String[] spell, String word) {
        for (String s : spell) {
            if (!isOnlyOneContain(s, word)) {
                return false;
            }
        }
        return true;
    }

    private boolean isOnlyOneContain(String spell, String word) {
        int count = 0;
        for (char character : word.toCharArray()) {
            if (spell.charAt(0) == character) {
                count++;
            }
        }
        return count == 1;
    }
}
~~~
