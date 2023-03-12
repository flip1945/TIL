# 공통 순열 (Silver 4)

### 문제 설명

알파벳 소문자로 이루어진 두 문자열 a와 b에 대해, a의 부분 수열의 순열이자 b의 부분 수열의 순열이 되는 가장 긴 문자열 x를 구하여라.

출처 : https://www.acmicpc.net/problem/1622

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str1 = "";
        while ((str1 = br.readLine()) != null) {
            String str2 = br.readLine();
            System.out.println(getCommonPermutation(str1, str2));
        }
    }

    static String getCommonPermutation(String str1, String str2) {
        String smallString = (str1.length() < str2.length()) ? str1 : str2;
        String largeString = (str1.length() < str2.length()) ? str2 : str1;
        List<String> smallStrings = getStringList(smallString);
        List<String> largeStrings = getStringList(largeString);

        return getCommonPermutationString(smallStrings, largeStrings);
    }

    static String getCommonPermutationString(List<String> smallStrings, List<String> largeStrings) {
        return smallStrings.stream()
                .filter(largeStrings::remove)
                .sorted()
                .reduce((a, b) -> a + b)
                .orElse("");
    }

    static List<String> getStringList(String string) {
        return Arrays.stream(string.split("")).collect(Collectors.toList());
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getCommonPermutationTest() {
        assertEquals("a", getCommonPermutation("a", "a"));
        assertEquals("b", getCommonPermutation("b", "b"));
        assertEquals("b", getCommonPermutation("ab", "b"));
        assertEquals("a", getCommonPermutation("a", "ab"));
        assertEquals("e", getCommonPermutation("pretty", "women"));
        assertEquals("nw", getCommonPermutation("walking", "down"));
        assertEquals("et", getCommonPermutation("the", "street"));
    }

    private String getCommonPermutation(String str1, String str2) {
        String smallString = (str1.length() < str2.length()) ? str1 : str2;
        String largeString = (str1.length() < str2.length()) ? str2 : str1;
        List<String> smallStrings = getStringList(smallString);
        List<String> largeStrings = getStringList(largeString);

        return getCommonPermutationString(smallStrings, largeStrings);
    }

    private String getCommonPermutationString(List<String> smallStrings, List<String> largeStrings) {
        return smallStrings.stream()
                .filter(largeStrings::remove)
                .sorted()
                .reduce((a, b) -> a + b)
                .orElse("");
    }

    private List<String> getStringList(String string) {
        return Arrays.stream(string.split("")).collect(Collectors.toList());
    }
}
~~~
