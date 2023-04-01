# 게임을 만든 동준이이 (Silver 4)

### 문제 설명
학교에서 그래픽스 수업을 들은 동준이는 수업시간에 들은 내용을 바탕으로 스마트폰 게임을 만들었다. 게임에는 총 N개의 레벨이 있고, 각 레벨을 클리어할 때 마다 점수가 주어진다. 플레이어의 점수는 레벨을 클리어하면서 얻은 점수의 합으로, 이 점수를 바탕으로 온라인 순위를 매긴다. 동준이는 레벨을 난이도 순으로 배치했다. 하지만, 실수로 쉬운 레벨이 어려운 레벨보다 점수를 많이 받는 경우를 만들었다.

이 문제를 해결하기 위해 동준이는 특정 레벨의 점수를 감소시키려고 한다. 이렇게해서 각 레벨을 클리어할 때 주는 점수가 증가하게 만들려고 한다.

각 레벨을 클리어할 때 얻는 점수가 주어졌을 때, 몇 번 감소시키면 되는지 구하는 프로그램을 작성하시오. 점수는 항상 양수이어야 하고, 1만큼 감소시키는 것이 1번이다. 항상 답이 존재하는 경우만 주어진다. 정답이 여러 가지인 경우에는 점수를 내리는 것을 최소한으로 하는 방법을 찾아야 한다.

출처 : https://www.acmicpc.net/problem/2847

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<Integer> levels = IntStream.range(0, n).mapToObj(i -> scanner.nextInt()).collect(Collectors.toList());

        System.out.println(dongJoonGame(levels));
    }

    static int dongJoonGame(List<Integer> levels) {
        int result = 0;
        int higherLevel = Integer.MAX_VALUE;
        for (int i = levels.size() - 1; i >= 0; i--) {
            result += getToBeReducedLevel(higherLevel, levels.get(i));
            higherLevel = nextHigherLevel(higherLevel, levels.get(i), getToBeReducedLevel(higherLevel, levels.get(i)));
        }
        return result;
    }

    static int nextHigherLevel(int higherLevel, int currentLevel, int toBeReducedLevel) {
        if (currentLevel >= higherLevel) {
            return currentLevel - toBeReducedLevel;
        }
        return currentLevel;
    }

    static int getToBeReducedLevel(int higherLevel, int currentLevel) {
        return Math.max(currentLevel - higherLevel + 1, 0);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void dongJoonGameTest() {
        assertEquals(3, dongJoonGame(List.of(5, 5, 5)));
        assertEquals(6, dongJoonGame(List.of(5, 5, 5, 5)));
        assertEquals(6, dongJoonGame(List.of(5, 3, 7, 5)));
        assertEquals(0, dongJoonGame(List.of(1, 2, 3, 4)));
        assertEquals(0, dongJoonGame(List.of(1, 20000)));
        assertEquals(10001, dongJoonGame(List.of(20000, 10000)));
    }

    private int dongJoonGame(List<Integer> levels) {
        int result = 0;
        int higherLevel = Integer.MAX_VALUE;
        for (int i = levels.size() - 1; i >= 0; i--) {
            result += getToBeReducedLevel(higherLevel, levels.get(i));
            higherLevel = nextHigherLevel(higherLevel, levels.get(i), getToBeReducedLevel(higherLevel, levels.get(i)));
        }
        return result;
    }

    private int nextHigherLevel(int higherLevel, int currentLevel, int toBeReducedLevel) {
        if (currentLevel >= higherLevel) {
            return currentLevel - toBeReducedLevel;
        }
        return currentLevel;
    }

    private int getToBeReducedLevel(int higherLevel, int currentLevel) {
        return Math.max(currentLevel - higherLevel + 1, 0);
    }
}
~~~
