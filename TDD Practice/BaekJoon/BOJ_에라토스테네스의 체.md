# 5와 6의 차이 (Silver 4)

### 문제 설명

에라토스테네스의 체는 N보다 작거나 같은 모든 소수를 찾는 유명한 알고리즘이다.

이 알고리즘은 다음과 같다.

1. 2부터 N까지 모든 정수를 적는다.
2. 아직 지우지 않은 수 중 가장 작은 수를 찾는다. 이것을 P라고 하고, 이 수는 소수이다.
3. P를 지우고, 아직 지우지 않은 P의 배수를 크기 순서대로 지운다.
4. 아직 모든 수를 지우지 않았다면, 다시 2번 단계로 간다.

N, K가 주어졌을 때, K번째 지우는 수를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/2960

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(getOrderOfSieveOfEratosthenes(scanner.nextInt(), scanner.nextInt()));
    }

    static int getOrderOfSieveOfEratosthenes(int n, int k) {
        List<Integer> orders = getOrders(n);
        return orders.get(k - 1);
    }

    static List<Integer> getOrders(int n) {
        List<Integer> orders = new ArrayList<>();
        int[] usedSieve = new int[n + 1];
        for (int i = 2; i <= n; i++) {
            addOrders(n, orders, usedSieve, i);
        }
        return orders;
    }

    static void addOrders(int n, List<Integer> orders, int[] usedSieve, int currentNumber) {
        if (isNotUsedSieve(usedSieve, currentNumber)) {
            addOrdersAndUseSieve(n, orders, usedSieve, currentNumber);
        }
    }

    static boolean isNotUsedSieve(int[] sieve, int number) {
        return sieve[number] == 0;
    }

    static void addOrdersAndUseSieve(int n, List<Integer> orders, int[] usedSieve, int prime) {
        for (int i = prime; i <= n; i += prime) {
            if (isNotUsedSieve(usedSieve, i)) {
                usedSieve[i] = 1;
                orders.add(i);
            }
        }
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getOrderOfSieveOfEratosthenesTest() {
        assertEquals(6, getOrderOfSieveOfEratosthenes(7, 3));
        assertEquals(2, getOrderOfSieveOfEratosthenes(2, 1));
        assertEquals(7, getOrderOfSieveOfEratosthenes(15, 12));
        assertEquals(9, getOrderOfSieveOfEratosthenes(10, 7));
    }

    private int getOrderOfSieveOfEratosthenes(int n, int k) {
        List<Integer> orders = getOrders(n);
        return orders.get(k - 1);
    }

    private List<Integer> getOrders(int n) {
        List<Integer> orders = new ArrayList<>();
        int[] usedSieve = new int[n + 1];
        for (int i = 2; i <= n; i++) {
            addOrders(n, orders, usedSieve, i);
        }
        return orders;
    }

    private void addOrders(int n, List<Integer> orders, int[] usedSieve, int currentNumber) {
        if (isNotUsedSieve(usedSieve, currentNumber)) {
            addOrdersAndUseSieve(n, orders, usedSieve, currentNumber);
        }
    }

    private boolean isNotUsedSieve(int[] sieve, int number) {
        return sieve[number] == 0;
    }

    private void addOrdersAndUseSieve(int n, List<Integer> orders, int[] usedSieve, int prime) {
        for (int i = prime; i <= n; i += prime) {
            if (isNotUsedSieve(usedSieve, i)) {
                usedSieve[i] = 1;
                orders.add(i);
            }
        }
    }
}
~~~
