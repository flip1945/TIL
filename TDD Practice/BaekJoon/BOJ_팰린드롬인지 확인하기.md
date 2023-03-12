# 팰린드롬인지 확인하기 (Bronze 2)

### 문제 설명

알파벳 소문자로만 이루어진 단어가 주어진다. 이때, 이 단어가 팰린드롬인지 아닌지 확인하는 프로그램을 작성하시오.

팰린드롬이란 앞으로 읽을 때와 거꾸로 읽을 때 똑같은 단어를 말한다. 

level, noon은 팰린드롬이고, baekjoon, online, judge는 팰린드롬이 아니다.

출처 : https://www.acmicpc.net/problem/10988

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(isPalindrome(scanner.nextLine()));
    }

    private static int isPalindrome(String word) {
        return word.equals(reverse(word)) ? 1 : 0;
    }

    private static String reverse(String word) {
        StringBuilder sb = new StringBuilder();
        for (int i = word.length() - 1; i >= 0; i--) {
            sb.append(word.charAt(i));
        }
        return sb.toString();
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
    void reverseTest() {
        String word1 = "level";
        String word2 = "baekjoon";
        String word3 = "a";
        String word4 = "ooooooooo";
        String word5 = "palindrome";

        assertEquals("level", reverse(word1));
        assertEquals("noojkeab", reverse(word2));
        assertEquals("a", reverse(word3));
        assertEquals("ooooooooo", reverse(word4));
        assertEquals("emordnilap", reverse(word5));
    }

    @Test
    void palindromeTest() {
        String word1 = "level";
        String word2 = "baekjoon";
        String word3 = "a";
        String word4 = "ooooooooo";
        String word5 = "palindrome";

        assertEquals(1, isPalindrome(word1));
        assertEquals(0, isPalindrome(word2));
        assertEquals(1, isPalindrome(word3));
        assertEquals(1, isPalindrome(word4));
        assertEquals(0, isPalindrome(word5));
    }

    private int isPalindrome(String word) {
        return word.equals(reverse(word)) ? 1 : 0;
    }

    private String reverse(String word) {
        StringBuilder sb = new StringBuilder();
        for (int i = word.length() - 1; i >= 0; i--) {
            sb.append(word.charAt(i));
        }
        return sb.toString();
    }
}
~~~
