# 문자열 (Silver 4)

### 문제 설명

길이가 N으로 같은 문자열 X와 Y가 있을 때, 두 문자열 X와 Y의 차이는 X[i] ≠ Y[i]인 i의 개수이다. 예를 들어, X=”jimin”, Y=”minji”이면, 둘의 차이는 4이다.

두 문자열 A와 B가 주어진다. 이때, A의 길이는 B의 길이보다 작거나 같다. 이제 A의 길이가 B의 길이와 같아질 때 까지 다음과 같은 연산을 할 수 있다.

1. A의 앞에 아무 알파벳이나 추가한다.
2. A의 뒤에 아무 알파벳이나 추가한다.

이때, A와 B의 길이가 같으면서, A와 B의 차이를 최소로 하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1120

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        System.out.println(getMinDiffCountOfString(st.nextToken(), st.nextToken()));
    }

    static int getMinDiffCountOfString(String a, String b) {
        int minDiffCountOfString = a.length();
        for (int i = 0; i < getDiffLength(a, b); i++) {
            minDiffCountOfString = Math.min(minDiffCountOfString, getDiffCountOfString(a, getSubstringOfB(a.length(), b, i)));
        }
        return minDiffCountOfString;
    }

    static int getDiffLength(String a, String b) {
        return b.length() - a.length() + 1;
    }

    static int getDiffCountOfString(String a, String b) {
        int diffCountOfString = 0;
        for (int i = 0; i < a.length(); i++) {
            if (a.charAt(i) != b.charAt(i)) {
                diffCountOfString++;
            }
        }
        return diffCountOfString;
    }

    static String getSubstringOfB(int aSize, String b, int startIndex) {
        return b.substring(startIndex, startIndex + aSize);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

public class MainTest {

    @Test
    void getMinDiffCountOfStringTest() {
        assertEquals(0, getMinDiffCountOfString("a", "abc"));
        assertEquals(1, getMinDiffCountOfString("ad", "abc"));
        assertEquals(0, getMinDiffCountOfString("bc", "abc"));
        assertEquals(3, getMinDiffCountOfString("def", "abc"));
        assertEquals(2, getMinDiffCountOfString("adaabc", "aababbc"));
        assertEquals(1, getMinDiffCountOfString("hello", "xello"));
        assertEquals(1, getMinDiffCountOfString("koder", "topcoder"));
        assertEquals(0, getMinDiffCountOfString("abc", "topabcoder"));
        assertEquals(6, getMinDiffCountOfString("giorgi", "igroig"));
    }

    private int getMinDiffCountOfString(String a, String b) {
        int minDiffCountOfString = a.length();
        for (int i = 0; i < getDiffLength(a, b); i++) {
            minDiffCountOfString = Math.min(minDiffCountOfString, getDiffCountOfString(a, getSubstringOfB(a.length(), b, i)));
        }
        return minDiffCountOfString;
    }

    private int getDiffLength(String a, String b) {
        return b.length() - a.length() + 1;
    }

    private int getDiffCountOfString(String a, String b) {
        int diffCountOfString = 0;
        for (int i = 0; i < a.length(); i++) {
            if (a.charAt(i) != b.charAt(i)) {
                diffCountOfString++;
            }
        }
        return diffCountOfString;
    }

    private String getSubstringOfB(int aSize, String b, int startIndex) {
        return b.substring(startIndex, startIndex + aSize);
    }
}
~~~
