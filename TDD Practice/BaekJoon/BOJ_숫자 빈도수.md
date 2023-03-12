# 숫자 빈도수 (Silver 5)

### 문제 설명

1부터 n까지 차례대로 써 내려갈 때 특정 숫자(digit)의 빈도수를 구하여 출력하는 프로그램을 작성하시오.

예를 들어, n = 11 이고 숫자 1의 빈도수를 구하라고 하면, 1 2 3 4 5 6 7 8 9 10 11 에서 숫자 1은 1에서 한 번, 10에서 한 번, 11에서 두 번 나타나므로 1의 빈도수는 총 4 이다.

출처 : https://www.acmicpc.net/problem/14912

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int d = scanner.nextInt();
        System.out.println(getCountOfNumber(n, d));
    }

    private static int getCountOfNumber(int n, int number) {
        int[] countOfNumbers = new int[]{0, 0, 0, 0, 0, 0, 0, 0, 0, 0};
        for (int i = 1; i <= n; i++) {
            countNumbers(String.valueOf(i), countOfNumbers);
        }
        return countOfNumbers[number];
    }

    private static int[] countNumbers(String numberString, int[] countOfNumbers) {
        for (String number : numberString.split("")) {
            int numberIndex = Integer.parseInt(number);
            countOfNumbers[numberIndex] += 1;
        }
        return countOfNumbers;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertArrayEquals;
import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void numberCountTest() {
        String number1 = "12345";
        String number2 = "1111";
        String number3 = "1";

        assertArrayEquals(new int[]{0, 1, 1, 1, 1, 1, 0, 0, 0, 0}, countNumbers(number1, new int[]{0, 0, 0, 0, 0, 0, 0, 0, 0, 0}));
        assertArrayEquals(new int[]{0, 4, 0, 0, 0, 0, 0, 0, 0, 0}, countNumbers(number2, new int[]{0, 0, 0, 0, 0, 0, 0, 0, 0, 0}));
        assertArrayEquals(new int[]{0, 1, 0, 0, 0, 0, 0, 0, 0, 0}, countNumbers(number3, new int[]{0, 0, 0, 0, 0, 0, 0, 0, 0, 0}));
        assertArrayEquals(new int[]{1, 2, 1, 1, 1, 1, 1, 1, 1, 1}, countNumbers(number3, new int[]{1, 1, 1, 1, 1, 1, 1, 1, 1, 1}));
    }

    @Test
    void integrationTest() {
        int n1 = 11;
        int n2 = 100;
        int n3 = 1;
        int number1 = 1;
        int number2 = 3;
        int number3 = 1;
        int number4 = 0;

        assertEquals(4, getCountOfNumber(n1, number1));
        assertEquals(20, getCountOfNumber(n2, number2));
        assertEquals(1, getCountOfNumber(n3, number3));
        assertEquals(1, getCountOfNumber(n1, number4));
    }

    private int getCountOfNumber(int n, int number) {
        int[] countOfNumbers = new int[]{0, 0, 0, 0, 0, 0, 0, 0, 0, 0};
        for (int i = 1; i <= n; i++) {
            countNumbers(String.valueOf(i), countOfNumbers);
        }
        return countOfNumbers[number];
    }

    private int[] countNumbers(String numberString, int[] countOfNumbers) {
        for (String number : numberString.split("")) {
            int numberIndex = Integer.parseInt(number);
            countOfNumbers[numberIndex] += 1;
        }
        return countOfNumbers;
    }
}
~~~
