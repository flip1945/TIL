# 블로그 (Silver 3)

### 문제 설명

찬솔이는 블로그를 시작한 지 벌써 
$N$일이 지났다.

요즘 바빠서 관리를 못 했다가 방문 기록을 봤더니 벌써 누적 방문 수가 6만을 넘었다.

<img src="https://upload.acmicpc.net/5f95a11c-b879-408b-b3be-dcaa915f36ab/-/preview/" width=600>


찬솔이는 $X$일 동안 가장 많이 들어온 방문자 수와 그 기간들을 알고 싶다.

찬솔이를 대신해서 $X$일 동안 가장 많이 들어온 방문자 수와 기간이 몇 개 있는지 구해주자.

출처 : https://www.acmicpc.net/problem/21921

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(bufferedReader.readLine());
        int n = Integer.parseInt(st.nextToken());
        int x = Integer.parseInt(st.nextToken());
        List<Integer> visitors = new ArrayList<>();
        st = new StringTokenizer(bufferedReader.readLine());
        for (int i = 0; i < n; i++) {
            visitors.add(Integer.parseInt(st.nextToken()));
        }

        int numberOfMostVisitorsInPeriod = getNumberOfMostVisitorsInPeriod(x, visitors);
        if (numberOfMostVisitorsInPeriod == 0) {
            System.out.println("SAD");
            return;
        }

        System.out.println(numberOfMostVisitorsInPeriod);
        System.out.println(getFrequentOfMostVisitorsInPeriod(x, visitors, numberOfMostVisitorsInPeriod));
    }

    static int getNumberOfMostVisitorsInPeriod(int period, List<Integer> visitors) {
        int[] prefixSum = getPrefixSum(visitors);
        int result = 0;
        for (int i = 0; i <= visitors.size() - period; i++) {
            result = Math.max(result, getSumOfVisitorsInPeriod(period, prefixSum, i));
        }
        return result;
    }

    static int[] getPrefixSum(List<Integer> visitors) {
        int[] prefixSum = new int[visitors.size() + 1];
        for (int i = 1; i <= visitors.size(); i++) {
            prefixSum[i] = prefixSum[i - 1] + visitors.get(i - 1);
        }
        return prefixSum;
    }

    static int getSumOfVisitorsInPeriod(int period, int[] prefixSum, int i) {
        return prefixSum[i + period] - prefixSum[i];
    }

    static int getFrequentOfMostVisitorsInPeriod(int period, List<Integer> visitors, int numberOfMostVisitorsInPeriod) {
        int[] prefixSum = getPrefixSum(visitors);
        int result = 0;
        for (int i = 0; i <= visitors.size() - period; i++) {
            if (numberOfMostVisitorsInPeriod == getSumOfVisitorsInPeriod(period, prefixSum, i)) {
                result++;
            }
        }
        return result;
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
    void getNumberOfMostVisitorsInPeriodTest() {
        assertEquals(7, getNumberOfMostVisitorsInPeriod(2, List.of(1, 4, 2, 5, 1)));
        assertEquals(9, getNumberOfMostVisitorsInPeriod(2, List.of(1, 2, 3, 4, 5)));
        assertEquals(15, getNumberOfMostVisitorsInPeriod(3, List.of(1, 2, 3, 4, 5, 6)));
        assertEquals(9, getNumberOfMostVisitorsInPeriod(5, List.of(1, 1, 1, 1, 1, 5, 1)));
        assertEquals(0, getNumberOfMostVisitorsInPeriod(3, List.of(0, 0, 0, 0, 0)));
    }

    @Test
    void getFrequentOfMostVisitorsInPeriodTest() {
        List<Integer> visitors1 = List.of(1, 4, 2, 5, 1);
        List<Integer> visitors2 = List.of(1, 2, 3, 4, 5);
        List<Integer> visitors3 = List.of(1, 1, 1, 1, 1, 5, 1);
        List<Integer> visitors4 = List.of(1, 2, 3, 4, 5, 6);
        List<Integer> visitors5 = List.of(1, 2, 1, 2, 1);
        assertEquals(1, getFrequentOfMostVisitorsInPeriod(2, visitors1, getNumberOfMostVisitorsInPeriod(2, visitors1)));
        assertEquals(1, getFrequentOfMostVisitorsInPeriod(2, visitors2, getNumberOfMostVisitorsInPeriod(2, visitors2)));
        assertEquals(2, getFrequentOfMostVisitorsInPeriod(5, visitors3, getNumberOfMostVisitorsInPeriod(5, visitors3)));
        assertEquals(1, getFrequentOfMostVisitorsInPeriod(3, visitors4, getNumberOfMostVisitorsInPeriod(3, visitors4)));
        assertEquals(4, getFrequentOfMostVisitorsInPeriod(2, visitors5, getNumberOfMostVisitorsInPeriod(2, visitors5)));
    }

    private int getNumberOfMostVisitorsInPeriod(int period, List<Integer> visitors) {
        int[] prefixSum = getPrefixSum(visitors);
        int result = 0;
        for (int i = 0; i <= visitors.size() - period; i++) {
            result = Math.max(result, getSumOfVisitorsInPeriod(period, prefixSum, i));
        }
        return result;
    }

    private int[] getPrefixSum(List<Integer> visitors) {
        int[] prefixSum = new int[visitors.size() + 1];
        for (int i = 1; i <= visitors.size(); i++) {
            prefixSum[i] = prefixSum[i - 1] + visitors.get(i - 1);
        }
        return prefixSum;
    }

    private int getSumOfVisitorsInPeriod(int period, int[] prefixSum, int i) {
        return prefixSum[i + period] - prefixSum[i];
    }

    private int getFrequentOfMostVisitorsInPeriod(int period, List<Integer> visitors, int numberOfMostVisitorsInPeriod) {
        int[] prefixSum = getPrefixSum(visitors);
        int result = 0;
        for (int i = 0; i <= visitors.size() - period; i++) {
            if (numberOfMostVisitorsInPeriod == getSumOfVisitorsInPeriod(period, prefixSum, i)) {
                result++;
            }
        }
        return result;
    }
}
~~~
