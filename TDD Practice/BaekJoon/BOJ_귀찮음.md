# 귀찮음 (Silver 5)

### 문제 설명

현우는 무슨 이유에선지 길이 a1, ..., an의, 총 n개의 쇠막대가 필요해졌다. 하지만 그가 가진 것은 길이 a1+...+an의 하나의 쇠막대뿐이었다. 현우는 이 막대를 직접 잘라서 원래 필요하던 n개의 쇠막대를 만들 것이다. 길이 x+y인 막대를 길이 x, y인 두 개의 막대로 자를 때에는 만들려 하는 두 막대의 길이의 곱인 xy의 비용이 든다. 현우는 최소의 비용으로 이 쇠막대를 잘라서 a1, ..., an의 n개의 쇠막대를 얻고 싶다.

그런데 현우는 이 비용이 얼마나 들지 잘 모르겠다. 그래서 여러분이 막대를 자르는 최소 비용을 계산하는 프로그램을 작성해주면 코드잼 경시대회 점수를 30점 올려주겠다고 제안했다. 어떤가?

출처 : https://www.acmicpc.net/problem/16208

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int[] rods = new int[n];
        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < n; i++) {
            rods[i] = Integer.parseInt(st.nextToken());
        }

        System.out.println(getTotalRodCost(rods));
    }

    static long getTotalRodCost(int[] rods) {
        if (rods.length == 1) {
            return rods[0];
        }

        return getEachRodCost(rods, getSizeOfTotalRods(rods));
    }

    static int getSizeOfTotalRods(int[] rods) {
        return Arrays.stream(rods).sum();
    }

    static long getEachRodCost(int[] rods, int sizeOfTotalRods) {
        Arrays.sort(rods);
        long answer = 0;
        for (int i = 0; i < rods.length - 1; i++) {
            sizeOfTotalRods -= rods[i];
            answer += (long) rods[i] * sizeOfTotalRods;
        }
        return answer;
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
    void getTotalRodCostTest() {
        assertEquals(2, getTotalRodCost(new int[]{1, 2}));
        assertEquals(6, getTotalRodCost(new int[]{2, 3}));
        assertEquals(11, getTotalRodCost(new int[]{2, 1, 3}));
        assertEquals(2, getTotalRodCost(new int[]{2}));
        assertEquals(71, getTotalRodCost(new int[]{3, 5, 4, 2}));
        assertEquals(55164, getTotalRodCost(new int[]{12, 43, 22, 51, 2, 55, 8, 21, 98, 50}));
    }

    private long getTotalRodCost(int[] rods) {
        if (rods.length == 1) {
            return rods[0];
        }

        return getEachRodCost(rods, getSizeOfTotalRods(rods));
    }

    private int getSizeOfTotalRods(int[] rods) {
        return Arrays.stream(rods).sum();
    }

    private long getEachRodCost(int[] rods, int sizeOfTotalRods) {
        Arrays.sort(rods);
        long answer = 0;
        for (int i = 0; i < rods.length - 1; i++) {
            sizeOfTotalRods -= rods[i];
            answer += (long) rods[i] * sizeOfTotalRods;
        }
        return answer;
    }
}
~~~
