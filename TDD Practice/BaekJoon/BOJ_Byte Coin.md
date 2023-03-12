# Byte Coin (Silver 4)

### 문제 설명

국제자본부동산회사(ICPC)는 바이트 코인(Byte Coin)에 자금을 투자하고 있다. 바이트 코인은 김박사가 만든 가상 화폐이다. 실제로는 바이트 코인 가격을 예상할 수 없지만 이 문제에서는 바이트 코인 가격 등락을 미리 정확히 예측할 수 있다고 가정하자.

우리는 1일부터 n일까지 n일 동안 그림 1과 같이 바이트 코인의 등락을 미리 알 수 있으며 우리에게는 초기 현금 W가 주어져 있다. 그림 1의 빨간색 네모는 해당 일자의 바이트 코인 가격을 나타낸다. 매일 바이트 코인을 매수하거나 매도할 수 있다고 하자. 다만 바이트 코인 하나를 나누어 매도하거나 매수할 수는 없다. 우리는 n일 날 보유하고 있는 모든 코인을 매도할 때 가지고 있는 현금을 최대화하고 싶다.

<img src="https://upload.acmicpc.net/4e5dc721-dfbb-4054-a545-713eee3137be/-/preview/" width=295px>

예를 들어, 그림 1과 같은 바이트 코인 등락 그래프를 첫날 미리 알 수 있다고 하고 우리에게 주어진 초기 현금이 24라고 하자. 수익을 최대한 높이려면 다음과 같이 바이트 코인을 매수, 매도할 수 있다. 첫 날 현금 20을 써서 4개의 코인을 산다. 둘째 날 모든 코인을 매도해서 현금 28을 얻고 모두 32의 현금을 갖게 된다. 5일째 되는 날 현금 32를 써서 16개의 코인을 매수한다. 7일째 되는 날 모든 코인을 매도해서 모두 128의 현금을 갖게 된다. 9일째 되는 날 현금 126을 써서 42개의 코인을 사고 10일 날 모든 코인을 매도한다. 그러면 10일 날 현금이 170이 되고 이것이 최대이다.

요일 수 n, 초기 현금 W, 1일부터 n일까지 각 요일의 바이트 코인 가격이 주어질 때, n일 날 보유하고 있는 모든 코인을 매도할 때 보유하게 될 최종 현금의 최댓값을 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/17521

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int money = scanner.nextInt();
        int[] byteCoinPrices = new int[n];
        for (int i = 0; i < n; i++) {
            byteCoinPrices[i] = scanner.nextInt();
        }
        System.out.println(getMaxProfit(n, money, byteCoinPrices));
    }

    static long getMaxProfit(int n, long money, int[] byteCoinPrices) {
        List<Integer> stack = new ArrayList<>();
        stack.add(byteCoinPrices[0]);
        for (int i = 1; i < n; i++) {
            money = calculateMoney(money, byteCoinPrices[i], stack);
            stack.add(byteCoinPrices[i]);
        }

        if (!stack.isEmpty()) {
            money = calculateProfit(money, stack.get(0), stack.get(stack.size() - 1));
        }
        return money;
    }

    static long calculateMoney(long money, int byteCoinPrice, List<Integer> stack) {
        if (isProfitable(byteCoinPrice, stack)) {
            money = calculateProfit(money, stack.get(0), stack.get(stack.size() - 1));
            stack.clear();
        }
        return money;
    }

    static boolean isProfitable(int byteCoinPrice, List<Integer> stack) {
        return stack.get(stack.size() - 1) > byteCoinPrice;
    }

    static long calculateProfit(long money, int lowCoinPrice, int highCoinPrice) {
        long coinCount = money / lowCoinPrice;
        long remainingMoney = money % lowCoinPrice;
        return highCoinPrice * coinCount + remainingMoney;
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
    void byteCoinTest() {
        assertEquals(20, getMaxProfit(2, 10, new int[]{1, 2}));
        assertEquals(30, getMaxProfit(3, 10, new int[]{1, 2, 3}));
        assertEquals(40, getMaxProfit(3, 10, new int[]{1, 4, 3}));
        assertEquals(20, getMaxProfit(3, 10, new int[]{2, 4, 3}));
        assertEquals(60, getMaxProfit(4, 10, new int[]{2, 4, 2, 6}));
        assertEquals(10, getMaxProfit(4, 10, new int[]{4, 3, 2, 1}));
        assertEquals(40, getMaxProfit(4, 10, new int[]{1, 4, 2, 1}));
        assertEquals(170, getMaxProfit(10, 24, new int[]{5, 7, 5, 4, 2, 7, 8, 5, 3, 4}));
        assertEquals(50, getMaxProfit(5, 15, new int[]{5, 4, 4, 2, 7}));
        assertEquals(54, getMaxProfit(7, 54, new int[]{7, 5, 5, 4, 2, 2, 1}));
        assertEquals(54, getMaxProfit(1, 54, new int[]{4}));
    }

    private long getMaxProfit(int n, long money, int[] byteCoinPrices) {
        List<Integer> stack = new ArrayList<>();
        stack.add(byteCoinPrices[0]);
        for (int i = 1; i < n; i++) {
            money = calculateMoney(money, byteCoinPrices[i], stack);
            stack.add(byteCoinPrices[i]);
        }

        if (!stack.isEmpty()) {
            money = calculateProfit(money, stack.get(0), stack.get(stack.size() - 1));
        }
        return money;
    }

    private long calculateMoney(long money, int byteCoinPrice, List<Integer> stack) {
        if (isProfitable(byteCoinPrice, stack)) {
            money = calculateProfit(money, stack.get(0), stack.get(stack.size() - 1));
            stack.clear();
        }
        return money;
    }

    private boolean isProfitable(int byteCoinPrice, List<Integer> stack) {
        return stack.get(stack.size() - 1) > byteCoinPrice;
    }

    private long calculateProfit(long money, int lowCoinPrice, int highCoinPrice) {
        long coinCount = money / lowCoinPrice;
        long remainingMoney = money % lowCoinPrice;
        return highCoinPrice * coinCount + remainingMoney;
    }
}
~~~
