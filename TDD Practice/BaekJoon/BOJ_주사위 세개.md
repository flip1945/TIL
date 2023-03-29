# 주사위 세개 (Bronze 4)

### 문제 설명

1에서부터 6까지의 눈을 가진 3개의 주사위를 던져서 다음과 같은 규칙에 따라 상금을 받는 게임이 있다. 

1. 같은 눈이 3개가 나오면 10,000원+(같은 눈)×1,000원의 상금을 받게 된다. 
2. 같은 눈이 2개만 나오는 경우에는 1,000원+(같은 눈)×100원의 상금을 받게 된다. 
3. 모두 다른 눈이 나오는 경우에는 (그 중 가장 큰 눈)×100원의 상금을 받게 된다.  

예를 들어, 3개의 눈 3, 3, 6이 주어지면 상금은 1,000+3×100으로 계산되어 1,300원을 받게 된다. 또 3개의 눈이 2, 2, 2로 주어지면 10,000+2×1,000 으로 계산되어 12,000원을 받게 된다. 3개의 눈이 6, 2, 5로 주어지면 그중 가장 큰 값이 6이므로 6×100으로 계산되어 600원을 상금으로 받게 된다.

3개 주사위의 나온 눈이 주어질 때, 상금을 계산하는 프로그램을 작성 하시오.

출처 : https://www.acmicpc.net/problem/2480

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(diceGame(scanner.nextInt(), scanner.nextInt(), scanner.nextInt()));
    }

    static int diceGame(int dice1, int dice2, int dice3) {
        if (areAllDiceSame(dice1, dice2, dice3)) {
            return dice1 * 1000 + 10000;
        } else if (areTwoDiceSame(dice1, dice2, dice3)) {
            return getSameDice(dice1, dice2, dice3) * 100 + 1000;
        }
        return getMaxDice(dice1, dice2, dice3) * 100;
    }

    static boolean areAllDiceSame(int dice1, int dice2, int dice3) {
        return dice1 == dice2 && dice2 == dice3;
    }

    static boolean areTwoDiceSame(int dice1, int dice2, int dice3) {
        return dice1 == dice2 || dice2 == dice3 || dice1 == dice3;
    }

    static int getSameDice(int dice1, int dice2, int dice3) {
        return (dice1 == dice2) ? dice1 : dice3;
    }

    static int getMaxDice(int dice1, int dice2, int dice3) {
        return Math.max(Math.max(dice1, dice2), dice3);
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
    void diceGameTest() {
        assertEquals(300, diceGame(1, 2, 3));
        assertEquals(400, diceGame(4, 2, 3));
        assertEquals(500, diceGame(1, 2, 5));
        assertEquals(600, diceGame(4, 6, 5));
        assertEquals(600, diceGame(6, 2, 5));
        assertEquals(1100, diceGame(1, 1, 5));
        assertEquals(1300, diceGame(3, 3, 6));
        assertEquals(1600, diceGame(6, 6, 2));
        assertEquals(16000, diceGame(6, 6, 6));
        assertEquals(13000, diceGame(3, 3, 3));
        assertEquals(12000, diceGame(2, 2, 2));
    }

    private int diceGame(int dice1, int dice2, int dice3) {
        if (areAllDiceSame(dice1, dice2, dice3)) {
            return dice1 * 1000 + 10000;
        } else if (areTwoDiceSame(dice1, dice2, dice3)) {
            return getSameDice(dice1, dice2, dice3) * 100 + 1000;
        }
        return getMaxDice(dice1, dice2, dice3) * 100;
    }

    private boolean areAllDiceSame(int dice1, int dice2, int dice3) {
        return dice1 == dice2 && dice2 == dice3;
    }

    private boolean areTwoDiceSame(int dice1, int dice2, int dice3) {
        return dice1 == dice2 || dice2 == dice3 || dice1 == dice3;
    }

    private int getSameDice(int dice1, int dice2, int dice3) {
        return (dice1 == dice2) ? dice1 : dice3;
    }

    private int getMaxDice(int dice1, int dice2, int dice3) {
        return Math.max(Math.max(dice1, dice2), dice3);
    }
}
~~~
