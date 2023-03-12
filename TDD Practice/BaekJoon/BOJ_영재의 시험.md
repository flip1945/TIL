# 영재의 시험 (Silver 3)

### 문제 설명

컴퓨터공학과 학생인 영재는 이번 학기에 알고리즘 수업을 수강한다.

평소에 자신의 실력을 맹신한 영재는 시험 전날까지 공부를 하지 않았다.

당연하게도 문제를 하나도 풀지 못하였지만 다행히도 문제가 5지 선다의 객관식 10문제였다.

찍기에도 자신 있던 영재는 3개의 연속된 문제의 답은 같지 않게 한다는 자신의 비법을 이용하여 모든 문제를 찍었다.

이때 영재의 점수가 5점 이상일 경우의 수를 구하여라.

문제의 점수는 1문제당 1점씩이다.

출처 : https://www.acmicpc.net/problem/19949

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int[] answers = new int[10];
        for (int i = 0; i < 10; i++) {
            answers[i] = scanner.nextInt();
        }
        System.out.println(solution(answers));
    }

    static int solution(int[] answers) {
        return backTracking(answers, new int[10], 0, 0);
    }

    static int backTracking(int[] answers, int[] submits, int currentDepth, int answer) {
        if (currentDepth >= 10) {
            if (isOver5Points(answers, submits)) {
                answer++;
            }
            return answer;
        }

        for (int i = 1; i <= 5; i++) {
            if (isCurrentDepthMoreThan2(currentDepth) && isSame3Numbers(i, submits[currentDepth - 1], submits[currentDepth - 2])) {
                continue;
            }
            submits[currentDepth] = i;
            answer = backTracking(answers, submits, currentDepth + 1, answer);
            submits[currentDepth] = 0;
        }
        return answer;
    }

    static boolean isCurrentDepthMoreThan2(int currentDepth) {
        return currentDepth >= 2;
    }

    static boolean isSame3Numbers(int number1, int number2, int number3) {
        return number1 == number2 && number1 == number3;
    }

    static boolean isOver5Points(int[] answers, int[] submits) {
        return IntStream.range(0, 10)
                .filter(i -> answers[i] == submits[i])
                .count() >= 5;
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
    void isOver5PointsTest() {
        assertEquals(true, isOver5Points(new int[]{1, 1, 1, 1, 1, 1, 1, 1, 1, 1}, new int[]{1, 1, 1, 1, 1, 2, 2, 2, 2, 2}));
        assertEquals(false, isOver5Points(new int[]{1, 1, 1, 1, 1, 1, 1, 1, 1, 1}, new int[]{2, 2, 2, 2, 2, 2, 2, 2, 2, 2}));
        assertEquals(false, isOver5Points(new int[]{1, 1, 1, 1, 1, 1, 1, 1, 1, 1}, new int[]{2, 2, 2, 2, 2, 2, 2, 3, 2, 1}));
        assertEquals(false, isOver5Points(new int[]{1, 1, 1, 1, 1, 1, 1, 1, 1, 1}, new int[]{2, 2, 2, 2, 2, 2, 2, 3, 1, 1}));
    }

    @Test
    void solutionTest() {
        assertEquals(261622, solution(new int[]{1, 2, 3, 4, 5, 1, 2, 3, 4, 5}));
    }

    private int solution(int[] answers) {
        return backTracking(answers, new int[10], 0, 0);
    }

    private int backTracking(int[] answers, int[] submits, int currentDepth, int answer) {
        if (currentDepth >= 10) {
            if (isOver5Points(answers, submits)) {
                answer++;
            }
            return answer;
        }

        for (int i = 1; i <= 5; i++) {
            if (isCurrentDepthMoreThan2(currentDepth) && isSame3Numbers(i, submits[currentDepth - 1], submits[currentDepth - 2])) {
                continue;
            }
            submits[currentDepth] = i;
            answer = backTracking(answers, submits, currentDepth + 1, answer);
            submits[currentDepth] = 0;
        }
        return answer;
    }

    private boolean isCurrentDepthMoreThan2(int currentDepth) {
        return currentDepth >= 2;
    }

    private boolean isSame3Numbers(int number1, int number2, int number3) {
        return number1 == number2 && number1 == number3;
    }

    private boolean isOver5Points(int[] answers, int[] submits) {
        return IntStream.range(0, 10)
                .filter(i -> answers[i] == submits[i])
                .count() >= 5;
    }
}
~~~
