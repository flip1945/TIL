# 주사위 네개 (Bronze 2)

### 문제 설명

1에서부터 6까지의 눈을 가진 4개의 주사위를 던져서 다음과 같은 규칙에 따라 상금을 받는 게임이 있다. 

1. 같은 눈이 4개가 나오면 50,000원+(같은 눈)×5,000원의 상금을 받게 된다. 
2. 같은 눈이 3개만 나오면 10,000원+(3개가 나온 눈)×1,000원의 상금을 받게 된다. 
3. 같은 눈이 2개씩 두 쌍이 나오는 경우에는 2,000원+(2개가 나온 눈)×500원+(또 다른 2개가 나온 눈)×500원의 상금을 받게 된다.
4. 같은 눈이 2개만 나오는 경우에는 1,000원+(같은 눈)×100원의 상금을 받게 된다. 
5. 모두 다른 눈이 나오는 경우에는 (그 중 가장 큰 눈)×100원의 상금을 받게 된다.  

예를 들어, 4개의 눈이 3, 3, 3, 3으로 주어지면 50,000+3×5,000으로 계산되어 65,000원의 상금을 받게 된다. 4개의 눈이 3, 3, 6, 3으로 주어지면 상금은 10,000+3×1,000으로 계산되어 13,000원을 받게 된다. 또 4개의 눈이 2, 2, 6, 6으로 주어지면 2,000+2×500+6×500으로 계산되어 6,000원을 받게 된다. 4개의 눈이 6, 2, 1, 6으로 주어지면 1,000+6×100으로 계산되어 1,600원을 받게 된다. 4개의 눈이 6, 2, 1, 5로 주어지면 그 중 가장 큰 값이 6이므로 6×100으로 계산되어 600원을 상금으로 받게 된다.

N(1 ≤ N ≤ 1,000)명이 주사위 게임에 참여하였을 때, 가장 많은 상금을 받은 사람의 상금을 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/2484

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int maxReward = 0;
        for (int i = 0; i < n; i++) {
            maxReward = Math.max(maxReward, calculateReward(List.of(scanner.nextInt(), scanner.nextInt(), scanner.nextInt(), scanner.nextInt())));
        }
        System.out.println(maxReward);
    }

    static int calculateReward(List<Integer> dices) {
        Map<Integer, Long> diceToCount = dices.stream()
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        switch (diceToCount.size()) {
            case 1: return getFirstReward(dices);
            case 2: return getSecondOrThirdReward(diceToCount);
            case 3: return getFourthReward(diceToCount);
            default: return getFifthReward(dices);
        }
    }

    static int getFirstReward(List<Integer> dices) {
        return 50000 + dices.get(0) * 5000;
    }

    static Integer getSecondOrThirdReward(Map<Integer, Long> diceToCount) {
        if (diceToCount.containsValue(3L)) {
            return getSecondReward(diceToCount);
        }
        return getThirdReward(diceToCount);
    }

    static Integer getSecondReward(Map<Integer, Long> diceToCount) {
        int sameDice = 0;
        for (Map.Entry<Integer, Long> entry : diceToCount.entrySet()) {
            if (entry.getValue() == 3) {
                sameDice = entry.getKey();
            }
        }
        return 10000 + sameDice * 1000;
    }

    static int getThirdReward(Map<Integer, Long> diceToCount) {
        int reward = 0;
        for (Map.Entry<Integer, Long> entry : diceToCount.entrySet()) {
            if (entry.getValue() == 2) {
                reward += 500 * entry.getKey();
            }
        }
        return reward + 2000;
    }

    static Integer getFourthReward(Map<Integer, Long> diceToCount) {
        int sameDice = 0;
        for (Map.Entry<Integer, Long> entry : diceToCount.entrySet()) {
            if (entry.getValue() == 2) {
                sameDice = entry.getKey();
            }
        }
        return 1000 + sameDice * 100;
    }

    static int getFifthReward(List<Integer> dices) {
        int maxDice = dices.stream().max(Comparator.naturalOrder()).orElse(0);
        return maxDice * 100;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void calculateRewardTest() {
        assertEquals(400, calculateReward(List.of(1, 2, 3, 4)));
        assertEquals(500, calculateReward(List.of(1, 2, 3, 5)));
        assertEquals(600, calculateReward(List.of(4, 2, 3, 6)));
        assertEquals(1300, calculateReward(List.of(4, 3, 3, 6)));
        assertEquals(1400, calculateReward(List.of(4, 4, 3, 6)));
        assertEquals(5500, calculateReward(List.of(4, 4, 3, 3)));
        assertEquals(7500, calculateReward(List.of(5, 5, 6, 6)));
        assertEquals(15000, calculateReward(List.of(5, 5, 5, 6)));
        assertEquals(16000, calculateReward(List.of(6, 6, 5, 6)));
        assertEquals(80000, calculateReward(List.of(6, 6, 6, 6)));
        assertEquals(65000, calculateReward(List.of(3, 3, 3, 3)));
    }

    private int calculateReward(List<Integer> dices) {
        Map<Integer, Long> diceToCount = dices.stream()
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        switch (diceToCount.size()) {
            case 1: return getFirstReward(dices);
            case 2: return getSecondOrThirdReward(diceToCount);
            case 3: return getFourthReward(diceToCount);
            default: return getFifthReward(dices);
        }
    }

    private int getFirstReward(List<Integer> dices) {
        return 50000 + dices.get(0) * 5000;
    }

    private Integer getSecondOrThirdReward(Map<Integer, Long> diceToCount) {
        if (diceToCount.containsValue(3L)) {
            return getSecondReward(diceToCount);
        }
        return getThirdReward(diceToCount);
    }

    private Integer getSecondReward(Map<Integer, Long> diceToCount) {
        int sameDice = 0;
        for (Map.Entry<Integer, Long> entry : diceToCount.entrySet()) {
            if (entry.getValue() == 3) {
                sameDice = entry.getKey();
            }
        }
        return 10000 + sameDice * 1000;
    }

    private int getThirdReward(Map<Integer, Long> diceToCount) {
        int reward = 0;
        for (Map.Entry<Integer, Long> entry : diceToCount.entrySet()) {
            if (entry.getValue() == 2) {
                reward += 500 * entry.getKey();
            }
        }
        return reward + 2000;
    }

    private Integer getFourthReward(Map<Integer, Long> diceToCount) {
        int sameDice = 0;
        for (Map.Entry<Integer, Long> entry : diceToCount.entrySet()) {
            if (entry.getValue() == 2) {
                sameDice = entry.getKey();
            }
        }
        return 1000 + sameDice * 100;
    }

    private int getFifthReward(List<Integer> dices) {
        int maxDice = dices.stream().max(Comparator.naturalOrder()).orElse(0);
        return maxDice * 100;
    }
}
~~~
