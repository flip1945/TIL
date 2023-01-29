# 합성수 찾기 (Level 0)

### 문제 설명

약수의 개수가 세 개 이상인 수를 합성수라고 합니다. 자연수 n이 매개변수로 주어질 때 n이하의 합성수의 개수를 return하도록 solution 함수를 완성해주세요.

#### 제한사항

* 1 ≤ n ≤ 100

출처 : https://programmers.co.kr/learn/courses/30/lessons/120846

---

#### 풀이

~~~java
import java.util.stream.IntStream;

class Solution {
    public int solution(int n) {
        int answer = 0;
        for (int i = 1; i <= n; i++) {
            if (isCompositeNumber(i)) {
                answer++;
            }
        }
        return answer;
    }

    private boolean isCompositeNumber(int number) {
        return IntStream.range(1, number).filter(i -> number % i == 0).count() > 1;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.stream.IntStream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void isCompositeNumberTest() {
        assertEquals(false, isCompositeNumber(1));
        assertEquals(false, isCompositeNumber(2));
        assertEquals(false, isCompositeNumber(3));
        assertEquals(true, isCompositeNumber(4));
        assertEquals(false, isCompositeNumber(5));
        assertEquals(true, isCompositeNumber(6));
        assertEquals(true, isCompositeNumber(10));
        assertEquals(false, isCompositeNumber(97));
        assertEquals(true, isCompositeNumber(100));
    }

    @Test
    void solutionTest() {
        int n1 = 10;
        int n2 = 15;
        int n3 = 1;

        assertEquals(5, solution(n1));
        assertEquals(8, solution(n2));
        assertEquals(0, solution(n3));
    }

    public int solution(int n) {
        int answer = 0;
        for (int i = 1; i <= n; i++) {
            if (isCompositeNumber(i)) {
                answer++;
            }
        }
        return answer;
    }

    private boolean isCompositeNumber(int number) {
        return IntStream.range(1, number).filter(i -> number % i == 0).count() > 1;
    }
}
~~~
