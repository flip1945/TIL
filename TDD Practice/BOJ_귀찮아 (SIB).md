# 귀찮아 (SIB) (Silver 5)

### 문제 설명

$\sum\limits_{1 \le a < b \le n}{x_ax_b}$

출처 : https://www.acmicpc.net/problem/14929

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int[] x = new int[n];
        for (int i = 0; i < n; i++) {
            x[i] = scanner.nextInt();
        }
        System.out.println(sib(n, x));
    }

    static long sib(int n, int[] x) {
        int[] prefixSum = getPrefixSum(n, x);
        return getSibSum(n, x, prefixSum);
    }

    static int[] getPrefixSum(int n, int[] x) {
        int[] prefixSum = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            prefixSum[i] = prefixSum[i - 1] + x[i - 1];
        }
        return prefixSum;
    }

    static long getSibSum(int n, int[] x, int[] prefixSum) {
        long sum = 0;
        for (int i = 0; i < n; i++) {
            sum += (long) x[i] * (prefixSum[n] - prefixSum[i + 1]);
        }
        return sum;
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
    void sibTest() {
        assertEquals(-5, sib(3, new int[]{1, -2, 3}));
        assertEquals(6, sib(2, new int[]{2, 3}));
        assertEquals(11, sib(3, new int[]{1, 2, 3}));
        assertEquals(35, sib(4, new int[]{1, 2, 3, 4}));
    }

    private long sib(int n, int[] x) {
        int[] prefixSum = getPrefixSum(n, x);
        return getSibSum(n, x, prefixSum);
    }

    private int[] getPrefixSum(int n, int[] x) {
        int[] prefixSum = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            prefixSum[i] = prefixSum[i - 1] + x[i - 1];
        }
        return prefixSum;
    }

    private long getSibSum(int n, int[] x, int[] prefixSum) {
        long sum = 0;
        for (int i = 0; i < n; i++) {
            sum += (long) x[i] * (prefixSum[n] - prefixSum[i + 1]);
        }
        return sum;
    }
}
~~~
