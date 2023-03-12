# 삼각형의 완성조건 (2) (Level 0)

### 문제 설명

선분 세 개로 삼각형을 만들기 위해서는 다음과 같은 조건을 만족해야 합니다.

가장 긴 변의 길이는 다른 두 변의 길이의 합보다 작아야 합니다.
삼각형의 두 변의 길이가 담긴 배열 sides이 매개변수로 주어집니다. 나머지 한 변이 될 수 있는 정수의 개수를 return하도록 solution 함수를 완성해주세요.

#### 제한사항

* sides의 원소는 자연수입니다.
* sides의 길이는 2입니다.
* 1 ≤ sides의 원소 ≤ 1,000

출처 : https://programmers.co.kr/learn/courses/30/lessons/120868

---

#### 풀이

~~~java
class Solution {
    public int solution(int[] sides) {
        int answer = 0;
        int longSide = Math.max(sides[0], sides[1]);
        int shortSide = Math.min(sides[0], sides[1]);
        answer += getAnswerIfLongSideIsLongest(longSide, shortSide);
        answer += getAnswerIfAnotherSideIsLongest(longSide, shortSide);
        return answer;
    }

    private int getAnswerIfAnotherSideIsLongest(int longSide, int shortSide) {
        return (longSide + shortSide) - longSide - 1;
    }

    private int getAnswerIfLongSideIsLongest(int longSide, int shortSide) {
        return longSide - (longSide - shortSide);
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
    void solutionTest() {
        int[] sides1 = new int[]{1, 2};
        int[] sides2 = new int[]{3, 6};
        int[] sides3 = new int[]{11, 7};
        int[] sides4 = new int[]{1, 1};
        int[] sides5 = new int[]{3, 3};

        assertEquals(1, solution(sides1));
        assertEquals(5, solution(sides2));
        assertEquals(13, solution(sides3));
        assertEquals(1, solution(sides4));
        assertEquals(5, solution(sides5));
    }

    public int solution(int[] sides) {
        int answer = 0;
        int longSide = Math.max(sides[0], sides[1]);
        int shortSide = Math.min(sides[0], sides[1]);
        answer += getAnswerIfLongSideIsLongest(longSide, shortSide);
        answer += getAnswerIfAnotherSideIsLongest(longSide, shortSide);
        return answer;
    }

    private int getAnswerIfAnotherSideIsLongest(int longSide, int shortSide) {
        return (longSide + shortSide) - longSide - 1;
    }

    private int getAnswerIfLongSideIsLongest(int longSide, int shortSide) {
        return longSide - (longSide - shortSide);
    }
}
~~~
