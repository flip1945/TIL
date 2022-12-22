# 연속된 수의 합 (Level 0)

### 문제 설명

연속된 세 개의 정수를 더해 12가 되는 경우는 3, 4, 5입니다. 두 정수 num과 total이 주어집니다. 연속된 수 num개를 더한 값이 total이 될 때, 정수 배열을 오름차순으로 담아 return하도록 solution함수를 완성해보세요.

#### 제한사항

* 1 ≤ num ≤ 100
* 0 ≤ total ≤ 1000
* num개의 연속된 수를 더하여 total이 될 수 없는 테스트 케이스는 없습니다.

출처 : https://programmers.co.kr/learn/courses/30/lessons/120923

---

#### 풀이

~~~java
import java.util.stream.IntStream;

public class Solution {
    public int[] solution(int n, int total) {
        int number = -n;
        while (calculateAverageNumber(n, number) != total) {
            number++;
        }
        return getResultInts(n, number);
    }

    private int[] getResultInts(int n, int startNumber) {
        int[] result = new int[n];
        for (int i = 0; i < n; i++) {
            result[i] = startNumber + i;
        }
        return result;
    }

    private int calculateAverageNumber(int num, int startNumber) {
        return IntStream.range(startNumber, startNumber + num).sum();
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.stream.IntStream;

import static org.junit.jupiter.api.Assertions.assertArrayEquals;
import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void calculateAverageNumberTest() {
        int n1 = 3;
        int n2 = 5;
        int startNumber1 = 3;
        int startNumber2 = 1;
        int startNumber3 = 1;
        int startNumber4 = -1;

        assertEquals(12, calculateAverageNumber(n1, startNumber1));
        assertEquals(6, calculateAverageNumber(n1, startNumber2));
        assertEquals(15, calculateAverageNumber(n2, startNumber3));
        assertEquals(5, calculateAverageNumber(n2, startNumber4));
    }

    @Test
    void solutionTest() {
        int n1 = 3;
        int n2 = 5;
        int n3 = 4;
        int n4 = 1;
        int total1 = 12;
        int total2 = 15;
        int total3 = 14;
        int total4 = 5;
        int total5 = 1;

        assertArrayEquals(new int[]{3, 4, 5}, solution(n1, total1));
        assertArrayEquals(new int[]{1, 2, 3, 4, 5}, solution(n2, total2));
        assertArrayEquals(new int[]{2, 3, 4, 5}, solution(n3, total3));
        assertArrayEquals(new int[]{-1, 0, 1, 2, 3}, solution(n2, total4));
        assertArrayEquals(new int[]{1}, solution(n4, total5));
    }

    private int[] solution(int n, int total) {
        int number = -n;
        while (calculateAverageNumber(n, number) != total) {
            number++;
        }
        return getResultInts(n, number);
    }

    private int[] getResultInts(int n, int startNumber) {
        int[] result = new int[n];
        for (int i = 0; i < n; i++) {
            result[i] = startNumber + i;
        }
        return result;
    }

    private int calculateAverageNumber(int num, int startNumber) {
        return IntStream.range(startNumber, startNumber + num).sum();
    }
}
~~~
