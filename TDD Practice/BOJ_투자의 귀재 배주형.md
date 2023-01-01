# 투자의 귀재 배주형 (Silver 5)

### 문제 설명

2020년에 학교로 복학한 주형이는 월세를 마련하기 위해서 군 적금을 깨고 복리 투자를 하려고 한다.

주형이가 하려는 투자에는 3가지 방법의 투자 방식이 있다. 

* 1년마다 5%의 이율을 얻는 투자 (A)
* 3년마다 20%의 이율을 얻는 투자 (B)
* 5년마다 35%의 이율을 얻는 투자 (C)

투자를 할 때에는 다음과 같은 주의점이 있다.

* 투자의 기한(1년, 3년, 5년)을 채우는 시점에 이율이 반영되며, 그 사이에는 돈이 늘어나지 않는다.
* 투자 방식은 매년 바꿀 수 있다.
* 매번 이율은 소수점 이하를 버림 해서 받는다.

예를 들어서, 지금 가진 돈이 11111원이면, A 방식이면 1년 후에 555원, B 방식이면 3년 후에 2,222원, C 방식이면 5년 후에 3,888원을 이자로 받을 수 있다. 만약 C 방식으로 투자했지만 4년이 지난 시점이라면 받을 수 있는 이자는 0원이다.

주형이의 초기 비용이 H원일 때, Y년이 지난 시점에 가장 많은 금액을 얻을 수 있는 투자 패턴을 분석하고 그 금액을 출력하자.

출처 : https://www.acmicpc.net/problem/19947

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    private static final Map<Integer, Double> rateMap = Map.of(1, 1.05, 3, 1.2, 5, 1.35);

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int money = scanner.nextInt();
        int year = scanner.nextInt();
        int[] dp = new int[15];
        dp[0] = money;

        for (int i = 0; i < 10; i++) {
            dp[i+1] = Math.max(dp[i+1], calculateInterest(dp[i], getRate(1)));
            dp[i+3] = Math.max(dp[i+3], calculateInterest(dp[i], getRate(3)));
            dp[i+5] = Math.max(dp[i+5], calculateInterest(dp[i], getRate(5)));
        }
        System.out.println(dp[year]);
    }

    private static int calculateInterest(int money, double rate) {
        return (int)(money * rate);
    }

    private static double getRate(int year) {
        return rateMap.get(year);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Map;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {
    private static final Map<Integer, Double> rateMap = Map.of(1, 1.05, 3, 1.2, 5, 1.35);

    @Test
    void calculateInterestTest() {
        int money1 = 10000;
        int money2 = 3333;
        int money3 = 95229;
        double rate1 = 1.05;
        double rate2 = 1.2;
        double rate3 = 1.35;

        assertEquals(10500, calculateInterest(money1, rate1));
        assertEquals(12000, calculateInterest(money1, rate2));
        assertEquals(13500, calculateInterest(money1, rate3));
        assertEquals(4499, calculateInterest(money2, rate3));
        assertEquals(114274, calculateInterest(money3, rate2));
    }

    @Test
    void getRateTest() {
        int year1 = 1;
        int year2 = 3;
        int year3 = 5;

        assertEquals(1.05, getRate(year1));
        assertEquals(1.2, getRate(year2));
        assertEquals(1.35, getRate(year3));
    }

    private double getRate(int year) {
        return rateMap.get(year);
    }

    private int calculateInterest(int money, double rate) {
        return (int)(money * rate);
    }
}
~~~
