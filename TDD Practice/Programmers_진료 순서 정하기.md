# 진료 순서 정하기 (Level 0)

### 문제 설명

외과의사 머쓱이는 응급실에 온 환자의 응급도를 기준으로 진료 순서를 정하려고 합니다. 정수 배열 emergency가 매개변수로 주어질 때 응급도가 높은 순서대로 진료 순서를 정한 배열을 return하도록 solution 함수를 완성해주세요.

#### 제한사항

* 중복된 원소는 없습니다.
* 1 ≤ emergency의 길이 ≤ 10
* 1 ≤ emergency의 원소 ≤ 100

출처 : https://programmers.co.kr/learn/courses/30/lessons/120835

---

#### 풀이

~~~java
import java.util.*;

class Solution {
    public List<Integer> solution(int[] emergency) {
        List<Integer> answer = new ArrayList<>();
        for (int i = 0; i < emergency.length; i++) {
            answer.add(getEmergencyRank(emergency, i));
        }
        return answer;
    }

    private int getEmergencyRank(int[] emergencyRates, int current) {
        int rank = 1;
        for (Integer emergencyRate : emergencyRates) {
            if (emergencyRates[current] < emergencyRate) {
                rank++;
            }
        }
        return rank;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void emergencyTest() {
        assertEquals(List.of(3, 1, 2), solution(new int[]{3, 76, 24}));
        assertEquals(List.of(7, 6, 5, 4, 3, 2, 1), solution(new int[]{1, 2, 3, 4, 5, 6, 7}));
        assertEquals(List.of(2, 4, 3, 5, 1), solution(new int[]{30, 10, 23, 6, 100}));
    }

    private List<Integer> solution(int[] emergency) {
        List<Integer> answer = new ArrayList<>();
        for (int i = 0; i < emergency.length; i++) {
            answer.add(getEmergencyRank(emergency, i));
        }
        return answer;
    }

    private int getEmergencyRank(int[] emergencyRates, int current) {
        int rank = 1;
        for (Integer emergencyRate : emergencyRates) {
            if (emergencyRates[current] < emergencyRate) {
                rank++;
            }
        }
        return rank;
    }
}
~~~
