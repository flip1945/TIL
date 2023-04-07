# 스위치와 램프 (Silver 4)

### 문제 설명

상도는 N개의 스위치와 M개의 램프를 갖고 있다. 스위치는 램프의 전원을 켤 수 있다. 스위치와 연결된 램프의 개수는 0개 이상이다.

가장 처음에 램프는 모두 꺼져 있다.

스위치를 누르면 램프의 전원이 켜진다. 스위치를 이용해서 램프의 전원을 끌 수는 없다. 예를 들어, 한 램프에 두 스위치가 연결되어 있는 경우에 한 스위치를 누르거나, 두 스위치를 모두 누르면 램프는 켜져 있는 상태가 된다.

N개의 스위치를 모두 누르면 모든 램프가 켜진다. 상도는 N-1개의 스위치를 눌러도 모든 램프가 켜지는지 궁금해졌다. 

스위치와 램프의 연결 상태가 입력으로 주어진다. N-1개의 스위치를 눌러서 모든 램프를 켤 수 있는지 알아보자.

출처 : https://www.acmicpc.net/problem/16960

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int m = scanner.nextInt();
        List<List<Integer>> switchAndLamps = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            int countOfSwitch = scanner.nextInt();
            switchAndLamps.add(IntStream.range(0, countOfSwitch)
                    .mapToObj(x -> scanner.nextInt())
                    .collect(Collectors.toList())
            );
        }

        System.out.println(switchAndLamp(m, switchAndLamps));
    }

    static int switchAndLamp(int countOfLamp, List<List<Integer>> switchAndLamps) {
        int[] lamps = initSwitchAndLamps(countOfLamp, switchAndLamps);
        return canOnAllLampOfExcludedOneSwitch(switchAndLamps, lamps);
    }

    static int[] initSwitchAndLamps(int countOfLamp, List<List<Integer>> switchAndLamps) {
        int[] lamps = new int[countOfLamp + 1];
        switchAndLamps.forEach(switchAndLamp -> switchAndLamp.forEach(switchToLamp -> lamps[switchToLamp]++));
        return lamps;
    }

    static int canOnAllLampOfExcludedOneSwitch(List<List<Integer>> switchAndLamps, int[] lamps) {
        for (List<Integer> switchAndLamp : switchAndLamps) {
            if (canOnAllLamp(lamps, switchAndLamp)) {
                return 1;
            }
        }
        return 0;
    }

    static boolean canOnAllLamp(int[] lamps, List<Integer> switchAndLamp) {
        return switchAndLamp.size() == switchAndLamp.stream().filter(i -> lamps[i] > 1).count();
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
    void switchAndLampTest() {
        assertEquals(0, switchAndLamp(3, List.of(List.of(1), List.of(2), List.of(3))));
        assertEquals(1, switchAndLamp(3, List.of(List.of(1, 2), List.of(2), List.of(3))));
        assertEquals(1, switchAndLamp(5, List.of(List.of(1, 3, 5), List.of(2), List.of(3, 4, 5), List.of(1))));
        assertEquals(0, switchAndLamp(5, List.of(List.of(1, 3), List.of(2), List.of(3, 4), List.of(3, 5))));
        assertEquals(1, switchAndLamp(1, List.of(List.of(1), List.of(), List.of(), List.of(1))));
    }

    private int switchAndLamp(int countOfLamp, List<List<Integer>> switchAndLamps) {
        int[] lamps = initSwitchAndLamps(countOfLamp, switchAndLamps);
        return canOnAllLampOfExcludedOneSwitch(switchAndLamps, lamps);
    }

    private int[] initSwitchAndLamps(int countOfLamp, List<List<Integer>> switchAndLamps) {
        int[] lamps = new int[countOfLamp + 1];
        switchAndLamps.forEach(switchAndLamp -> switchAndLamp.forEach(switchToLamp -> lamps[switchToLamp]++));
        return lamps;
    }

    private int canOnAllLampOfExcludedOneSwitch(List<List<Integer>> switchAndLamps, int[] lamps) {
        for (List<Integer> switchAndLamp : switchAndLamps) {
            if (canOnAllLamp(lamps, switchAndLamp)) {
                return 1;
            }
        }
        return 0;
    }

    private boolean canOnAllLamp(int[] lamps, List<Integer> switchAndLamp) {
        return switchAndLamp.size() == switchAndLamp.stream().filter(i -> lamps[i] > 1).count();
    }
}
~~~
