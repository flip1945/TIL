# 팰린드로미터 (Silver 5)

### 문제 설명

승환이는 팰린드롬을 좋아한다. 지금 승환이의 자동차의 주행 거리계에 100000이 적혀있다. 승환이는 1km만 더 주행을 하면 100001이 된다! 승환이는 엄청나게 흥분했다.

주행 거리계에 적혀져 있는 수가 주어졌을 때, 몇 km를 더 주행하면 팰린드롬이 되는지 구하는 프로그램을 작성하시오. 승환이는 모든 자리가 팰린드롬이 되어야 한다. 따라서, 주행 거리계에 00121인 경우에는 팰린드롬이 아니다.

출처 : https://www.acmicpc.net/problem/4096

---

#### 풀이
~~~java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        String n = bf.readLine();
        while (!n.equals("0")) {
            System.out.println(getPalindrometer(n));
            n = bf.readLine();
        }
    }

    private static int getPalindrometer(String s) {
        int number = Integer.parseInt(s);
        for (int i = number; i < 1000000000; i++) {
            if (isPalindrome(zFill(s, i))) {
                return i - number;
            }
        }
        return -1;
    }

    private static String zFill(String s, int number) {
        String tmp = String.valueOf(number);
        return "0".repeat(Math.max(0, s.length() - tmp.length())) + tmp;
    }

    private static boolean isPalindrome(String s) {
        int stringSize = s.length();
        for (int i = 0; i < stringSize / 2; i++) {
            if (s.charAt(i) != s.charAt(stringSize - i - 1)) {
                return false;
            }
        }
        return true;
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
    void isPalindromeTest() {
        assertEquals(true, isPalindrome("100001"));
        assertEquals(false, isPalindrome("100000"));
        assertEquals(true, isPalindrome("0"));
        assertEquals(false, isPalindrome("10"));
        assertEquals(true, isPalindrome("99"));
    }

    @Test
    void getPalindrometerTest() {
        assertEquals(1, getPalindrometer("100000"));
        assertEquals(0, getPalindrometer("100001"));
        assertEquals(979, getPalindrometer("000121"));
        assertEquals(44, getPalindrometer("00456"));
        assertEquals(109, getPalindrometer("0111"));
    }

    private int getPalindrometer(String s) {
        int number = Integer.parseInt(s);
        for (int i = number; i < 1000000000; i++) {
            if (isPalindrome(zFill(s, i))) {
                return i - number;
            }
        }
        return -1;
    }

    private String zFill(String s, int number) {
        String tmp = String.valueOf(number);
        return "0".repeat(Math.max(0, s.length() - tmp.length())) + tmp;
    }

    private boolean isPalindrome(String s) {
        int stringSize = s.length();
        for (int i = 0; i < stringSize / 2; i++) {
            if (s.charAt(i) != s.charAt(stringSize - i - 1)) {
                return false;
            }
        }
        return true;
    }
}
~~~
