# 유한소수 판별하기 (Level 0)

### 문제 설명

소수점 아래 숫자가 계속되지 않고 유한개인 소수를 유한소수라고 합니다. 분수를 소수로 고칠 때 유한소수로 나타낼 수 있는 분수인지 판별하려고 합니다. 유한소수가 되기 위한 분수의 조건은 다음과 같습니다.

* 기약분수로 나타내었을 때, 분모의 소인수가 2와 5만 존재해야 합니다.

두 정수 a와 b가 매개변수로 주어질 때, a/b가 유한소수이면 1을, 무한소수라면 2를 return하도록 solution 함수를 완성해주세요.

#### 제한사항

* a, b는 정수
* 0 < a ≤ 1,000
* 0 < b ≤ 1,000

출처 : https://programmers.co.kr/learn/courses/30/lessons/120878

---

#### 풀이

~~~java
import java.util.*;

class Solution {
    public int solution(int a, int b) {
        int[] irreducibleFraction = getIrreducibleFraction(a, b);
        List<Integer> factors = getFactorization(irreducibleFraction[1]);
        return isFiniteDecimal(factors);
    }

    private int[] getIrreducibleFraction(int a, int b) {
        int min = Math.min(a, b);
        int i = 2;
        while (i <= min) {
            if (canDivided(a, b, i)) {
                a /= i;
                b /= i;
                continue;
            }
            i++;
        }
        return new int[]{a, b};
    }

    private boolean canDivided(int a, int b, int i) {
        return a % i == 0 && b % i == 0;
    }

    private List<Integer> getFactorization(int number) {
        List<Integer> factors = new ArrayList<>();
        int limit = number;
        int i = 2;
        while (i <= limit) {
            if (number % i == 0) {
                number /= i;
                factors.add(i);
                continue;
            }
            i++;
        }
        return factors;
    }

    private int isFiniteDecimal(List<Integer> factors) {
        for (Integer factor : factors) {
            if (factor != 2 && factor != 5) {
                return 2;
            }
        }
        return 1;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertArrayEquals;
import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getIrreducibleFractionTest() {
        int a1 = 10;
        int a2 = 7;
        int a3 = 0;
        int b1 = 20;
        int b2 = 0;

        assertArrayEquals(new int[]{1, 2}, getIrreducibleFraction(a1, b1));
        assertArrayEquals(new int[]{7, 20}, getIrreducibleFraction(a2, b1));
        assertArrayEquals(new int[]{0, 0}, getIrreducibleFraction(a3, b2));
        assertArrayEquals(new int[]{7, 0}, getIrreducibleFraction(a2, b2));
        assertArrayEquals(new int[]{10, 0}, getIrreducibleFraction(a1, b2));
    }

    @Test
    void factorizationTest() {
        int number1 = 20;
        int number2 = 1000;
        int number3 = 0;
        int number4 = 21;
        int number5 = 22;

        assertEquals(List.of(2, 2, 5), getFactorization(number1));
        assertEquals(List.of(2, 2, 2, 5, 5, 5), getFactorization(number2));
        assertEquals(List.of(), getFactorization(number3));
        assertEquals(List.of(3, 7), getFactorization(number4));
        assertEquals(List.of(2, 11), getFactorization(number5));
    }

    @Test
    void isFiniteDecimalTest() {
        List<Integer> factors1 = List.of(2, 2, 5);
        List<Integer> factors2 = List.of(2, 11);
        List<Integer> factors3 = List.of();
        List<Integer> factors4 = List.of(2, 2, 2, 5, 5, 5);

        assertEquals(1, isFiniteDecimal(factors1));
        assertEquals(2, isFiniteDecimal(factors2));
        assertEquals(1, isFiniteDecimal(factors3));
        assertEquals(1, isFiniteDecimal(factors4));
    }

    @Test
    void solutionTest() {
        int a1 = 7;
        int a2 = 12;
        int a3 = 11;
        int a4 = 1000;
        int b1 = 20;
        int b2 = 21;
        int b3 = 22;
        int b4 = 1000;

        assertEquals(1, solution(a1, b1));
        assertEquals(2, solution(a2, b2));
        assertEquals(1, solution(a3, b3));
        assertEquals(1, solution(a4, b4));
    }

    public int solution(int a, int b) {
        int[] irreducibleFraction = getIrreducibleFraction(a, b);
        List<Integer> factors = getFactorization(irreducibleFraction[1]);
        return isFiniteDecimal(factors);
    }

    private int[] getIrreducibleFraction(int a, int b) {
        int min = Math.min(a, b);
        int i = 2;
        while (i <= min) {
            if (canDivided(a, b, i)) {
                a /= i;
                b /= i;
                continue;
            }
            i++;
        }
        return new int[]{a, b};
    }

    private boolean canDivided(int a, int b, int i) {
        return a % i == 0 && b % i == 0;
    }

    private List<Integer> getFactorization(int number) {
        List<Integer> factors = new ArrayList<>();
        int limit = number;
        int i = 2;
        while (i <= limit) {
            if (number % i == 0) {
                number /= i;
                factors.add(i);
                continue;
            }
            i++;
        }
        return factors;
    }

    private int isFiniteDecimal(List<Integer> factors) {
        for (Integer factor : factors) {
            if (factor != 2 && factor != 5) {
                return 2;
            }
        }
        return 1;
    }
}
~~~
