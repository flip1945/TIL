# 다항식 더하기 (Level 0)

### 문제 설명

머쓱이는 구슬을 친구들에게 나누어주려고 합니다. 구슬은 모두 다르게 생겼습니다. 머쓱이가 갖고 있는 구슬의 개수 balls와 친구들에게 나누어 줄 구슬 개수 share이 매개변수로 주어질 때, balls개의 구슬 중 share개의 구슬을 고르는 가능한 모든 경우의 수를 return 하는 solution 함수를 완성해주세요.

#### 제한사항

* 1 ≤ balls ≤ 30
* 1 ≤ share ≤ 30
* 구슬을 고르는 순서는 고려하지 않습니다.
* share ≤ balls

출처 : https://programmers.co.kr/learn/courses/30/lessons/120840

---

#### 풀이

~~~java
import java.math.BigInteger;

class Solution {
    public long solution(int balls, int share) {
        if (balls == share) {
            return 1;
        }
        return combination(balls, share).longValue();
    }

    private BigInteger combination(int total, int pick) {
        pick = Math.min(pick, total - pick);
        return permutation(total, pick).divide(new BigInteger(String.valueOf(factorial(pick))));
    }

    private BigInteger permutation(int total, int pick) {
        BigInteger number = new BigInteger("1");
        for (int i = total; i > total - pick; i--) {
            number = number.multiply(new BigInteger(String.valueOf(i)));
        }
        return number;
    }

    private long factorial(int i) {
        if (i == 1) {
            return 1;
        }
        return i * factorial(i - 1);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.math.BigInteger;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void factorialTest() {
        assertEquals(1, factorial(1));
        assertEquals(2, factorial(2));
        assertEquals(6, factorial(3));
        assertEquals(24, factorial(4));
        assertEquals(120, factorial(5));
        assertEquals(1307674368000L, factorial(15));
    }

    @Test
    void permutationTest() {
        assertEquals(new BigInteger("1"), permutation(1, 1));
        assertEquals(new BigInteger("60"), permutation(5, 3));
        assertEquals(new BigInteger("90"), permutation(10, 2));
        assertEquals(new BigInteger("15"), permutation(15, 1));
        assertEquals(new BigInteger("202843204931727360000"), permutation(30, 15));
    }

    @Test
    void combinationTest() {
        assertEquals(new BigInteger("3"), combination(3, 2));
        assertEquals(new BigInteger("10"), combination(5, 3));
        assertEquals(new BigInteger("10"), combination(10, 1));
        assertEquals(new BigInteger("155117520"), combination(30, 15));
        assertEquals(new BigInteger("1"), combination(30, 30));
    }

    private BigInteger combination(int total, int pick) {
        pick = Math.min(pick, total - pick);
        return permutation(total, pick).divide(new BigInteger(String.valueOf(factorial(pick))));
    }

    private BigInteger permutation(int total, int pick) {
        BigInteger number = new BigInteger("1");
        for (int i = total; i > total - pick; i--) {
            number = number.multiply(new BigInteger(String.valueOf(i)));
        }
        return number;
    }

    private long factorial(int i) {
        if (i == 1) {
            return 1;
        }
        return i * factorial(i - 1);
    }
}
~~~
