# 부분 문자열 (Silver 5)

### 문제 설명

2개의 문자열 s와 t가 주어졌을 때 s가 t의 부분 문자열인지 판단하는 프로그램을 작성하라. 부분 문자열을 가지고 있는지 판단하는 방법은 t에서 몇 개의 문자를 제거하고 이를 순서를 바꾸지 않고 합쳤을 경우 s가 되는 경우를 이야기 한다.

출처 : https://www.acmicpc.net/problem/6550

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String input;
        StringTokenizer st;
        while ((input = br.readLine()) != null) {
            st = new StringTokenizer(input);
            System.out.println(isSubString(st.nextToken(), st.nextToken()));
        }
    }

    private static String isSubString(String string, String totalString) {
        int stringIndex = 0;
        int stringSize = string.length();
        int totalStringIndex = 0;
        int totalStringSize = totalString.length();

        while (hasNext(stringIndex, stringSize, totalStringIndex, totalStringSize)) {
            stringIndex = getStringIndex(string, stringIndex, totalString, totalStringIndex);
            if (stringIndex == stringSize) {
                return "Yes";
            }
            totalStringIndex++;
        }
        return "No";
    }

    private static boolean hasNext(int stringIndex, int stringSize, int totalStringIndex, int totalStringSize) {
        return stringIndex < stringSize && totalStringIndex < totalStringSize;
    }

    private static int getStringIndex(String string, int stringIndex, String totalString, int totalStringIndex) {
        if (string.charAt(stringIndex) == totalString.charAt(totalStringIndex)) {
            stringIndex++;
        }
        return stringIndex;
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
    void isSubStringTest() {
        assertEquals("Yes", isSubString("s", "s"));
        assertEquals("No", isSubString("s", "a"));
        assertEquals("Yes", isSubString("s", "sa"));
        assertEquals("Yes", isSubString("sb", "sab"));
        assertEquals("Yes", isSubString("sequence", "subsequence"));
        assertEquals("No", isSubString("person", "compression"));
        assertEquals("Yes", isSubString("VERDI", "vivaVittorioEmanueleReDiItalia"));
        assertEquals("No", isSubString("caseDoesMatter", "CaseDoesMatter"));
    }

    private String isSubString(String string, String totalString) {
        int stringIndex = 0;
        int stringSize = string.length();
        int totalStringIndex = 0;
        int totalStringSize = totalString.length();

        while (hasNext(stringIndex, stringSize, totalStringIndex, totalStringSize)) {
            stringIndex = getStringIndex(string, stringIndex, totalString, totalStringIndex);
            if (stringIndex == stringSize) {
                return "Yes";
            }
            totalStringIndex++;
        }
        return "No";
    }

    private boolean hasNext(int stringIndex, int stringSize, int totalStringIndex, int totalStringSize) {
        return stringIndex < stringSize && totalStringIndex < totalStringSize;
    }

    private int getStringIndex(String string, int stringIndex, String totalString, int totalStringIndex) {
        if (string.charAt(stringIndex) == totalString.charAt(totalStringIndex)) {
            stringIndex++;
        }
        return stringIndex;
    }
}
~~~
