# 치킨 쿠폰 (Level 0)

### 문제 설명

프로그래머스 치킨은 치킨을 시켜먹으면 한 마리당 쿠폰을 한 장 발급합니다. 쿠폰을 열 장 모으면 치킨을 한 마리 서비스로 받을 수 있고, 서비스 치킨에도 쿠폰이 발급됩니다. 시켜먹은 치킨의 수 chicken이 매개변수로 주어질 때 받을 수 있는 최대 서비스 치킨의 수를 return하도록 solution 함수를 완성해주세요.

#### 제한사항

* chicken은 정수입니다.
* 0 ≤ chicken ≤ 1,000,000

출처 : https://programmers.co.kr/learn/courses/30/lessons/120884

---

#### 풀이

~~~java
public class Solution {
    public int solution(int chicken) {
        int answer = 0;
        while (chicken >= 10) {
            answer += chicken / 10;
            int extraChicken = chicken % 10;
            chicken = (chicken / 10) + extraChicken;
        }
        return answer;
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
    void test() {
        int chicken1 = 1;
        int chicken2 = 10;
        int chicken3 = 100;
        int chicken4 = 1081;

        assertEquals(0, solution(chicken1));
        assertEquals(1, solution(chicken2));
        assertEquals(11, solution(chicken3));
        assertEquals(120, solution(chicken4));
    }

    private int solution(int chicken) {
        int answer = 0;
        while (chicken >= 10) {
            answer += chicken / 10;
            int extraChicken = chicken % 10;
            chicken = (chicken / 10) + extraChicken;
        }
        return answer;
    }
}
~~~
