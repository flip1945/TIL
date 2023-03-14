# 후보 추천하기 (Silver 1)

### 문제 설명

월드초등학교 학생회장 후보는 일정 기간 동안 전체 학생의 추천에 의하여 정해진 수만큼 선정된다. 그래서 학교 홈페이지에 추천받은 학생의 사진을 게시할 수 있는 사진틀을 후보의 수만큼 만들었다. 추천받은 학생의 사진을 사진틀에 게시하고 추천받은 횟수를 표시하는 규칙은 다음과 같다.

1. 학생들이 추천을 시작하기 전에 모든 사진틀은 비어있다.
2. 어떤 학생이 특정 학생을 추천하면, 추천받은 학생의 사진이 반드시 사진틀에 게시되어야 한다.
3. 비어있는 사진틀이 없는 경우에는 현재까지 추천 받은 횟수가 가장 적은 학생의 사진을 삭제하고, 그 자리에 새롭게 추천받은 학생의 사진을 게시한다. 이때, 현재까지 추천 받은 횟수가 가장 적은 학생이 두 명 이상일 경우에는 그러한 학생들 중 게시된 지 가장 오래된 사진을 삭제한다.
4. 현재 사진이 게시된 학생이 다른 학생의 추천을 받은 경우에는 추천받은 횟수만 증가시킨다.
5. 사진틀에 게시된 사진이 삭제되는 경우에는 해당 학생이 추천받은 횟수는 0으로 바뀐다.

후보의 수 즉, 사진틀의 개수와 전체 학생의 추천 결과가 추천받은 순서대로 주어졌을 때, 최종 후보가 누구인지 결정하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1713

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int c = Integer.parseInt(br.readLine());
        int[] recommends = Arrays.stream(br.readLine().split(" "))
                .mapToInt(Integer::parseInt).toArray();

        recommendCandidate(n, recommends).forEach(num -> System.out.print(num + " "));
    }

    static List<Integer> recommendCandidate(int countOfFrame, int[] recommends) {
        List<Integer> frame = new ArrayList<>();
        int[] countOfRecommended = new int[101];
        for (int recommend : recommends) {
            recommendAndPushFrame(countOfFrame, frame, countOfRecommended, recommend);
        }
        frame.sort(Comparator.naturalOrder());
        return frame;
    }

    static void recommendAndPushFrame(int countOfFrame, List<Integer> frame, int[] countOfRecommended, int recommend) {
        if (containsRecommendStudentInFrame(frame, recommend)) {
            countOfRecommended[recommend]++;
            return;
        }
        removeLowestRecommendedOrOldest(countOfFrame, frame, countOfRecommended);
        frame.add(recommend);
    }

    static boolean containsRecommendStudentInFrame(List<Integer> frame, int recommend) {
        return frame.contains(recommend);
    }

    static void removeLowestRecommendedOrOldest(int countOfFrame, List<Integer> frame, int[] countOfRecommended) {
        if (frame.size() == countOfFrame) {
            int removeRecommend = findLowestOrOldestRecommend(frame, countOfRecommended);
            countOfRecommended[removeRecommend] = 0;
            frame.remove(Integer.valueOf(removeRecommend));
        }
    }

    static int findLowestOrOldestRecommend(List<Integer> frame, int[] countOfRecommended) {
        int min = 1001;
        int removeRecommend = -1;
        for (int student : frame) {
            if (countOfRecommended[student] < min) {
                min = countOfRecommended[student];
                removeRecommend = student;
            }
        }
        return removeRecommend;
    }
}

~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void recommendCandidateTest() {
        assertEquals(List.of(1), recommendCandidate(1, new int[]{1}));
        assertEquals(List.of(2), recommendCandidate(1, new int[]{2}));
        assertEquals(List.of(2), recommendCandidate(1, new int[]{1, 2}));
        assertEquals(List.of(1), recommendCandidate(1, new int[]{2, 1}));
        assertEquals(List.of(1, 2), recommendCandidate(2, new int[]{2, 1}));
        assertEquals(List.of(1, 3), recommendCandidate(2, new int[]{2, 1, 3}));
        assertEquals(List.of(2, 3), recommendCandidate(2, new int[]{1, 2, 2, 1, 3}));
        assertEquals(List.of(1, 2, 4), recommendCandidate(3, new int[]{1, 1, 1, 2, 2, 3, 4}));
        assertEquals(List.of(2, 3, 4), recommendCandidate(3, new int[]{1, 1, 2, 2, 3, 3, 4}));
        assertEquals(List.of(2, 6, 7), recommendCandidate(3, new int[]{2, 1, 4, 3, 5, 6, 2, 7, 2}));
        assertEquals(List.of(3, 4), recommendCandidate(2, new int[]{1, 1, 2, 2, 3, 3, 1, 4}));
    }

    private List<Integer> recommendCandidate(int countOfFrame, int[] recommends) {
        List<Integer> frame = new ArrayList<>();
        int[] countOfRecommended = new int[101];
        for (int recommend : recommends) {
            recommendAndPushFrame(countOfFrame, frame, countOfRecommended, recommend);
        }
        frame.sort(Comparator.naturalOrder());
        return frame;
    }

    private void recommendAndPushFrame(int countOfFrame, List<Integer> frame, int[] countOfRecommended, int recommend) {
        if (containsRecommendStudentInFrame(frame, recommend)) {
            countOfRecommended[recommend]++;
            return;
        }
        removeLowestRecommendedOrOldest(countOfFrame, frame, countOfRecommended);
        frame.add(recommend);
    }

    private boolean containsRecommendStudentInFrame(List<Integer> frame, int recommend) {
        return frame.contains(recommend);
    }

    private void removeLowestRecommendedOrOldest(int countOfFrame, List<Integer> frame, int[] countOfRecommended) {
        if (frame.size() == countOfFrame) {
            int removeRecommend = findLowestOrOldestRecommend(frame, countOfRecommended);
            countOfRecommended[removeRecommend] = 0;
            frame.remove(Integer.valueOf(removeRecommend));
        }
    }

    private int findLowestOrOldestRecommend(List<Integer> frame, int[] countOfRecommended) {
        int min = 1001;
        int removeRecommend = -1;
        for (int student : frame) {
            if (countOfRecommended[student] < min) {
                min = countOfRecommended[student];
                removeRecommend = student;
            }
        }
        return removeRecommend;
    }
}
~~~
