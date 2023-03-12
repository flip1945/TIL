# 등수 매기기 (Level 0)

### 문제 설명

영어 점수와 수학 점수의 평균 점수를 기준으로 학생들의 등수를 매기려고 합니다. 영어 점수와 수학 점수를 담은 2차원 정수 배열 score가 주어질 때, 영어 점수와 수학 점수의 평균을 기준으로 매긴 등수를 담은 배열을 return하도록 solution 함수를 완성해주세요.

#### 제한사항

* 0 ≤ score[0], score[1] ≤ 100
* 1 ≤ score의 길이 ≤ 10
* score의 원소 길이는 2입니다.
* score는 중복된 원소를 갖지 않습니다.

출처 : https://programmers.co.kr/learn/courses/30/lessons/120882

---

#### 풀이

~~~java
import java.util.*;
import java.util.stream.Collectors;

class Solution {
    public int[] solution(int[][] score) {
        List<Integer> rank = Arrays.stream(score)
                .map(this::sumArray)
                .sorted(Comparator.reverseOrder())
                .collect(Collectors.toList());

        int[] answer = new int[score.length];
        for (int i = 0; i < score.length; i++) {
            answer[i] = rank.indexOf(sumArray(score[i])) + 1;
        }
        return answer;
    }

    private int sumArray(int[] array) {
        return array[0] + array[1];
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

import static org.junit.jupiter.api.Assertions.assertArrayEquals;
import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void arraySumTest() {
        int[] array1 = new int[]{80, 70};
        int[] array2 = new int[]{90, 50};
        int[] array3 = new int[]{100, 100};
        int[] array4 = new int[]{0, 0};

        assertEquals(150, sumArray(array1));
        assertEquals(140, sumArray(array2));
        assertEquals(200, sumArray(array3));
        assertEquals(0, sumArray(array4));
    }

    @Test
    void solutionTest() {
        int[][] score1 = new int[][]{{80, 70}, {90, 50}, {40, 70}, {50, 80}};
        int[][] score2 = new int[][]{{80, 70}, {70, 80}, {30, 50}, {90, 100}, {100, 90}, {100, 100}, {10, 30}};
        int[][] score3 = new int[][]{{10, 0}, {10, 0}, {10, 0}, {10, 0}, {10, 0}, {0, 0}};

        assertArrayEquals(new int[]{1, 2, 4, 3}, solution(score1));
        assertArrayEquals(new int[]{4, 4, 6, 2, 2, 1, 7}, solution(score2));
        assertArrayEquals(new int[]{1, 1, 1, 1, 1, 6}, solution(score3));
    }

    private int[] solution(int[][] score) {
        List<Integer> rank = Arrays.stream(score)
                .map(this::sumArray)
                .sorted(Comparator.reverseOrder())
                .collect(Collectors.toList());

        int[] answer = new int[score.length];
        for (int i = 0; i < score.length; i++) {
            answer[i] = rank.indexOf(sumArray(score[i])) + 1;
        }
        return answer;
    }

    private int sumArray(int[] array) {
        return array[0] + array[1];
    }
}
~~~
