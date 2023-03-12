# 다항식 더하기 (Level 0)

### 문제 설명

첫 번째 분수의 분자와 분모를 뜻하는 numer1, denom1, 두 번째 분수의 분자와 분모를 뜻하는 numer2, denom2가 매개변수로 주어집니다. 두 분수를 더한 값을 기약 분수로 나타냈을 때 분자와 분모를 순서대로 담은 배열을 return 하도록 solution 함수를 완성해보세요.

#### 제한사항

* 0 <numer1, denom1, numer2, denom2 < 1,000

출처 : https://programmers.co.kr/learn/courses/30/lessons/120808

---

#### 풀이

~~~java
import java.util.List;

class Solution {
    public List<Integer> solution(int numer1, int denom1, int numer2, int denom2) {
        List<Integer> sumOfFraction = sumFraction(numer1, denom1, numer2, denom2);
        int numerator = sumOfFraction.get(0);
        int denominator = sumOfFraction.get(1);
        int gcd = gcd(numerator, denominator);
        return List.of(numerator / gcd, denominator / gcd);
    }

    private List<Integer> sumFraction(int numer1, int denom1, int numer2, int denom2) {
        return List.of((numer1 * denom2) + (numer2 * denom1), denom1 * denom2);
    }

    private int gcd(int n1, int n2) {
        if (n1 % n2 == 0) {
            return n2;
        }
        return gcd(n2, n1 % n2);
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
    void sumFractionTest() {
        assertEquals(List.of(10, 8), sumFraction(1, 2, 3, 4));
        assertEquals(List.of(29, 6), sumFraction(9, 2, 1, 3));
        assertEquals(List.of(2, 1), sumFraction(1, 1, 1, 1));
        assertEquals(List.of(4, 4), sumFraction(1, 2, 1, 2));
        assertEquals(List.of(8, 4), sumFraction(2, 2, 2, 2));
    }

    @Test
    void gcdTest() {
        assertEquals(2, gcd(10, 8));
        assertEquals(2, gcd(8, 10));
        assertEquals(1, gcd(29, 6));
        assertEquals(1, gcd(2, 1));
        assertEquals(4, gcd(4, 4));
        assertEquals(4, gcd(8, 4));
        assertEquals(1, gcd(3, 7));
    }

    @Test
    void solutionTest() {
        assertEquals(List.of(5, 4), solution(1, 2, 3, 4));
        assertEquals(List.of(29, 6), solution(9, 2, 1, 3));
        assertEquals(List.of(2, 1), solution(1, 1, 1, 1));
        assertEquals(List.of(1, 1), solution(1, 2, 1, 2));
        assertEquals(List.of(2, 1), solution(2, 2, 2, 2));
    }

    public List<Integer> solution(int numer1, int denom1, int numer2, int denom2) {
        List<Integer> sumOfFraction = sumFraction(numer1, denom1, numer2, denom2);
        int numerator = sumOfFraction.get(0);
        int denominator = sumOfFraction.get(1);
        int gcd = gcd(numerator, denominator);
        return List.of(numerator / gcd, denominator / gcd);
    }

    private List<Integer> sumFraction(int numer1, int denom1, int numer2, int denom2) {
        return List.of((numer1 * denom2) + (numer2 * denom1), denom1 * denom2);
    }

    private int gcd(int n1, int n2) {
        if (n1 % n2 == 0) {
            return n2;
        }
        return gcd(n2, n1 % n2);
    }
}
~~~
