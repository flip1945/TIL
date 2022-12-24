# 복호화 (Bronze 2)

### 문제 설명

암호학에서 치환 암호(substitution cipher)란, 평문에 들어있는 각각의 문자를 주어진 치환 방법으로 암호화하는 방법 중 하나다.

가장 단순한 방법은 평문의 알파벳을 암호문의 알파벳으로 대치시켜 치환시키는 것이다.

예를 들어, 아래와 같은 알파벳 대치표가 주어졌다고 하자.

* 평문 알파벳 대치표 : abcdefghijklmnopqrstuvwxyz
* 암호문 알파벳 대치표 : wghuvijxpqrstacdebfklmnoyz

위에 주어진 치환 방법을 통해 암호화하면 평문 "hello there"은 "xvssc kxvbv"가 된다.

한 가지 흥미로운 점은 영어 문법 특성상, 알파벳 'e'가 다른 영문 알파벳에 비해 자주 쓰인다는 것이다.

즉, 암호문 알파벳 대치표 없이 암호문을 복호화하려 할 때, 암호문 알파벳 빈도수를 체크하면 암호문 알파벳 빈도수 중 가장 빈번하게 나타나는 알파벳이 'e'라는 사실을 유추해볼 수 있다.

위 방법으로 암호문 알파벳의 빈도수를 체크하고, 가장 빈번하게 나타나는 문자를 출력하는 프로그램을 작성하면 된다.

만약 주어진 암호문에서 가장 빈번하게 나타나는 문자가 여러 개일 경우, 그 빈번한 문자 중 어느 것이 평문 알파벳 'e'를 가리키는지 확실하게 알 수 없기 때문에 "모르겠음"을 의미하는 '?'를 출력하면 된다.

출처 : https://www.acmicpc.net/problem/9046

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < t; i++) {
            System.out.println(getMostFrequentLetter(scanner.nextLine()));
        }
    }

    private static String getMostFrequentLetter(String word) {
        int[] countOfLetters = getCountOfLetters(word.replaceAll(" ", ""));
        int maxCount = getMaxCount(countOfLetters);
        return getFrequentLetter(countOfLetters, maxCount);
    }

    private static int[] getCountOfLetters(String word) {
        int[] countOfLetters = new int[26];
        for (char c : word.toCharArray()) {
            countOfLetters[c-'a']++;
        }
        return countOfLetters;
    }

    private static Integer getMaxCount(int[] countOfLetters) {
        return Collections.max(Arrays.stream(countOfLetters).boxed().collect(Collectors.toList()));
    }

    private static String getFrequentLetter(int[] countOfLetters, int max) {
        String frequentLetter = "";
        for (int i = 0; i < countOfLetters.length; i++) {
            frequentLetter = getFrequentLetterOrQuestionMark(countOfLetters, max, frequentLetter, i);
            if (frequentLetter == null) return "?";
        }
        return frequentLetter;
    }

    private static String getFrequentLetterOrQuestionMark(int[] countOfLetters, int max, String frequentLetter, int i) {
        if (countOfLetters[i] == max) {
            if (!frequentLetter.equals("")) {
                return null;
            }
            frequentLetter = String.valueOf((char)(i + 97));
        }
        return frequentLetter;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Arrays;
import java.util.Collections;
import java.util.stream.Collectors;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getMostFrequentLetterTest() {
        String word1 = "aaad";
        String word2 = "bbba";
        String word3 = "   a";
        String word4 = "aabb";
        String word5 = "asvdge ef ofmdofn";
        String word6 = "xvssc kxvbv";
        String word7 = "hull full suua pmlu";

        assertEquals("a", getMostFrequentLetter(word1));
        assertEquals("b", getMostFrequentLetter(word2));
        assertEquals("a", getMostFrequentLetter(word3));
        assertEquals("?", getMostFrequentLetter(word4));
        assertEquals("f", getMostFrequentLetter(word5));
        assertEquals("v", getMostFrequentLetter(word6));
        assertEquals("?", getMostFrequentLetter(word7));
    }

    private String getMostFrequentLetter(String word) {
        int[] countOfLetters = getCountOfLetters(word.replaceAll(" ", ""));
        int maxCount = getMaxCount(countOfLetters);
        return getFrequentLetter(countOfLetters, maxCount);
    }

    private int[] getCountOfLetters(String word) {
        int[] countOfLetters = new int[26];
        for (char c : word.toCharArray()) {
            countOfLetters[c-'a']++;
        }
        return countOfLetters;
    }

    private Integer getMaxCount(int[] countOfLetters) {
        return Collections.max(Arrays.stream(countOfLetters).boxed().collect(Collectors.toList()));
    }

    private String getFrequentLetter(int[] countOfLetters, int max) {
        String frequentLetter = "";
        for (int i = 0; i < countOfLetters.length; i++) {
            frequentLetter = getFrequentLetterOrQuestionMark(countOfLetters, max, frequentLetter, i);
            if (frequentLetter == null) return "?";
        }
        return frequentLetter;
    }

    private String getFrequentLetterOrQuestionMark(int[] countOfLetters, int max, String frequentLetter, int i) {
        if (countOfLetters[i] == max) {
            if (!frequentLetter.equals("")) {
                return null;
            }
            frequentLetter = String.valueOf((char)(i + 97));
        }
        return frequentLetter;
    }
}
~~~
