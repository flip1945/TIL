# 성적 통계 (Silver 5)

### 문제 설명

한상덕은 이번에 중덕 고등학교에 새로 부임한 교장 선생님이다. 교장 선생님으로서 첫 번째 일은 각 반의 수학 시험 성적의 통계를 내는 일이다.

중덕 고등학교 각 반의 학생들의 수학 시험 성적이 주어졌을 때, 최대 점수, 최소 점수, 점수 차이를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/5800

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int k = scanner.nextInt();
        for (int i = 1; i <= k; i++) {
            int scoreSize = scanner.nextInt();
            List<Integer> scores = IntStream.range(0, scoreSize)
                    .mapToObj(num -> scanner.nextInt())
                    .collect(Collectors.toList());

            List<Integer> gradeStatistics = getGradeStatistics(scores);

            printStatistics(i, gradeStatistics);
        }
    }

    private static void printStatistics(int i, List<Integer> gradeStatistics) {
        System.out.println("Class " + i);
        System.out.println("Max " + gradeStatistics.get(0) + ", Min " + gradeStatistics.get(1) + ", Largest gap " + gradeStatistics.get(2));
    }

    static List<Integer> getGradeStatistics(List<Integer> scores) {
        List<Integer> sortedScore = getSortedScore(scores);
        return List.of(getMaxScore(sortedScore), getMinScore(sortedScore), getLargestGap(sortedScore));
    }

    static List<Integer> getSortedScore(List<Integer> scores) {
        return scores.stream()
                .sorted()
                .collect(Collectors.toList());
    }

    static Integer getMaxScore(List<Integer> sortedScore) {
        return sortedScore.get(sortedScore.size()-1);
    }

    static Integer getMinScore(List<Integer> sortedScore) {
        return sortedScore.get(0);
    }

    static int getLargestGap(List<Integer> sortedScore) {
        int result = 0;
        for (int i = 0; i < sortedScore.size() - 1; i++) {
            result = Math.max(result, sortedScore.get(i + 1) - sortedScore.get(i));
        }
        return result;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.List;
import java.util.stream.Collectors;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getGradeStatisticsTest() {
        assertEquals(List.of(4, 1, 2), getGradeStatistics(List.of(2, 4, 1)));
        assertEquals(List.of(8, 2, 3), getGradeStatistics(List.of(2, 4, 8, 7)));
        assertEquals(List.of(78, 23, 46), getGradeStatistics(List.of(30, 25, 76, 23, 78)));
        assertEquals(List.of(99, 25, 25), getGradeStatistics(List.of(25, 50, 70, 99, 70, 90)));
    }

    private List<Integer> getGradeStatistics(List<Integer> scores) {
        List<Integer> sortedScore = getSortedScore(scores);
        return List.of(getMaxScore(sortedScore), getMinScore(sortedScore), getLargestGap(sortedScore));
    }

    private List<Integer> getSortedScore(List<Integer> scores) {
        return scores.stream()
                .sorted()
                .collect(Collectors.toList());
    }

    private Integer getMaxScore(List<Integer> sortedScore) {
        return sortedScore.get(sortedScore.size()-1);
    }

    private Integer getMinScore(List<Integer> sortedScore) {
        return sortedScore.get(0);
    }

    private int getLargestGap(List<Integer> sortedScore) {
        int result = 0;
        for (int i = 0; i < sortedScore.size() - 1; i++) {
            result = Math.max(result, sortedScore.get(i + 1) - sortedScore.get(i));
        }
        return result;
    }
}
~~~
