# 간판 (Silver 5)

### 문제 설명

상근이는 학교 근처에 새로운 편의점을 열었다. 편의점의 얼굴은 간판이라고 할 수 있다. 상근이가 새로 연 편의점은 프랜차이즈 편의점이 아니기 때문에, 간판도 자신이 직접 돈을 들여서 만들어야 한다.

근처 편의점은 이미 할인 카드, 적립 카드와 같은 정책으로 손님을 끌어 모으고 있다. 상근이는 전 품목을 5%해서 손님을 모으려고 한다. 이렇게 물건의 가격을 할인해서 팔려면, 다른 곳에 들어가는 비용을 줄어야 한다. 따라서, 상근이는 간판을 재활용해서 만들기로 했다.

편의점이 있기 전에 원래 이 곳은 간판 가게였다. 따라서, 편의점에는 이전 주인이 버리고 간 오래된 간판이 N개 있다. 상근이는 오래된 간판에서 일부 문자를 지워 새로운 간판을 만들려고 한다. 이때, 남은 문자열이 편의점 이름이어야 하고, 남은 문자가 모두 일정한 간격으로 떨어져 있어야 한다. 간판은 오래된 간판 하나에서 만들어야 하고, 간판을 자르거나 붙일수는 없다.

편의점 이름과 오래된 간판의 정보가 주어졌을 때, 만들 수 있는 새 간판의 수를 구하는 프로그램을 작성하시오. 하나의 오래된 간판에서 만들 수 있는 방법이 여러 개인 경우에도 만들 수 있는 간판은 하나이다.

출처 : https://www.acmicpc.net/problem/5534

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        String newSign = scanner.nextLine();
        String[] originSigns = new String[n];
        for (int i = 0; i < n; i++) {
            originSigns[i] = scanner.nextLine();
        }

        System.out.println(getChangeableSignCount(newSign, originSigns));
    }

    static int getChangeableSignCount(String newSign, String[] originSigns) {
        return Arrays.stream(originSigns).mapToInt(originSign -> isChangeable(newSign, originSign)).sum();
    }
    
    static int isChangeable(String newSign, String originSign) {
        int newSignSize = newSign.length();
        int originSignSize = originSign.length();

        for (int interval = 1; interval <= originSignSize / (newSignSize - 1); interval++) {
            for (int startIndex = 0; startIndex < originSignSize; startIndex++) {
                if (newSign.equals(createSign(originSign, newSignSize, originSignSize, interval, startIndex))) {
                    return 1;
                }
            }
        }
        return 0;
    }

    static String createSign(String originSign, int newSignSize, int originSignSize, int interval, int startIndex) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < newSignSize; i++) {
            int index = startIndex + (interval * i);
            if (index < originSignSize) {
                sb.append(originSign.charAt(index));
            }
        }
        return sb.toString();
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Arrays;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getChangeableSignCountTest() {
        assertEquals(4, getChangeableSignCount("abc", new String[]{"abc", "a b c ", "a  b c", " a b c", "a  b  c"}));
        assertEquals(3, getChangeableSignCount("bar", new String[]{"abracadabra", "bear", "bar", "baraxbara"}));
        assertEquals(4, getChangeableSignCount("word", new String[]{"word", " w o r d", "  w  o  r  d   ", "w    o    r    d"}));
    }

    private int getChangeableSignCount(String newSign, String[] originSigns) {
        return Arrays.stream(originSigns).mapToInt(originSign -> isChangeable(newSign, originSign)).sum();
    }

    private int isChangeable(String newSign, String originSign) {
        int newSignSize = newSign.length();
        int originSignSize = originSign.length();

        for (int interval = 1; interval <= originSignSize / (newSignSize - 1); interval++) {
            for (int startIndex = 0; startIndex < originSignSize; startIndex++) {
                if (newSign.equals(createSign(originSign, newSignSize, originSignSize, interval, startIndex))) {
                    return 1;
                }
            }
        }
        return 0;
    }

    private String createSign(String originSign, int newSignSize, int originSignSize, int interval, int startIndex) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < newSignSize; i++) {
            int index = startIndex + (interval * i);
            if (index < originSignSize) {
                sb.append(originSign.charAt(index));
            }
        }
        return sb.toString();
    }
}
~~~
