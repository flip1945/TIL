# 비숌 사이 수열 (Silver 2)

### 문제 설명

숌은 N개의 다른 숫자로 구성되어 있는 집합 X를 만들었다. 그리고, 길이가 2N인 숌 사이 수열 (S)을 만들려고 한다.

숌 사이 수열이란 다음과 같다.

1. X에 들어있는 모든 수는 숌 사이 수열 S에 정확히 두 번 등장해야 한다.
2. X에 등장하는 수가 i라면, S에서 두 번 등장하는 i사이에는 수가 i개 등장해야 한다.

예를 들어, 숌이 만든 집합 X가 {1,2,3}이고, 숌이 만든 숌 사이 수열이 {2 3 1 2 1 3}이라면, 일단 X에 속하는 모든 수가 S에 두 번 등장하므로 1번 조건을 만족한다. 그리고, 2와 2사이엔 수가 두 개, 1과 1사이엔 1개, 3과 3사이엔 3개가 등장하므로 조건을 만족시킨다.

집합 X가 주어졌을 때, 숌 사이 수열 S를 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1469

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        int[] numbers = new int[n];
        for (int i = 0; i < n; i++) {
            numbers[i] = Integer.parseInt(st.nextToken());
        }
        System.out.println(solution(n, numbers));
    }

    static String solution(int n, int[] numbers) {
        String answer = backTracking("", n * 2, getTwiceNumbers(numbers), new ArrayList<>(), new boolean[n * 2]);
        return (answer.equals("")) ? "-1" : answer;
    }

    static List<Integer> getTwiceNumbers(int[] numbers) {
        Arrays.sort(numbers);
        List<Integer> twiceNumbers = new ArrayList<>();
        for (int number : numbers) {
            twiceNumbers.add(number);
            twiceNumbers.add(number);
        }
        return twiceNumbers;
    }

    static String backTracking(String answer, int n, List<Integer> totalNumbers, List<Integer> shomNumbers, boolean[] used) {
        if (!answer.equals("")) {
            return answer;
        }
        if (shomNumbers.size() == n) {
            return shomNumbers.stream().map(String::valueOf).collect(Collectors.joining(" "));
        }
        for (int i = 0; i < n; i++) {
            int currentNumber = totalNumbers.get(i);
            if (used[i] || shomNumbers.contains(currentNumber) && shomNumbers.size() - currentNumber - 1 != shomNumbers.indexOf(currentNumber) || (!shomNumbers.contains(currentNumber) && shomNumbers.size() + currentNumber >= n)) {
                continue;
            }
            used[i] = true;
            shomNumbers.add(currentNumber);
            answer = backTracking(answer, n, totalNumbers, shomNumbers, used);
            shomNumbers.remove(shomNumbers.size() - 1);
            used[i] = false;
        }
        return answer;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void integrationTest() {
        assertEquals("2 3 1 2 1 3", solution(3, new int[]{1, 2, 3}));
        assertEquals("0 0", solution(1, new int[]{0}));
        assertEquals("2 3 4 2 1 3 1 4", solution(4, new int[]{1, 2, 3, 4}));
        assertEquals("-1", solution(5, new int[]{1, 2, 3, 4, 5}));
        assertEquals("2 0 0 2", solution(2, new int[]{2, 0}));
        assertEquals("-1", solution(8, new int[]{0, 4, 13, 12, 8, 5, 2, 14}));
    }

    private String solution(int n, int[] numbers) {
        String answer = backTracking("", n * 2, getTwiceNumbers(numbers), new ArrayList<>(), new boolean[n * 2]);
        return (answer.equals("")) ? "-1" : answer;
    }

    private List<Integer> getTwiceNumbers(int[] numbers) {
        Arrays.sort(numbers);
        List<Integer> twiceNumbers = new ArrayList<>();
        for (int number : numbers) {
            twiceNumbers.add(number);
            twiceNumbers.add(number);
        }
        return twiceNumbers;
    }

    private String backTracking(String answer, int n, List<Integer> totalNumbers, List<Integer> shomNumbers, boolean[] used) {
        if (!answer.equals("")) {
            return answer;
        }
        if (shomNumbers.size() == n) {
            return shomNumbers.stream().map(String::valueOf).collect(Collectors.joining(" "));
        }
        for (int i = 0; i < n; i++) {
            int currentNumber = totalNumbers.get(i);
            if (used[i] || shomNumbers.contains(currentNumber) && shomNumbers.size() - currentNumber - 1 != shomNumbers.indexOf(currentNumber) || (!shomNumbers.contains(currentNumber) && shomNumbers.size() + currentNumber >= n)) {
                continue;
            }
            used[i] = true;
            shomNumbers.add(currentNumber);
            answer = backTracking(answer, n, totalNumbers, shomNumbers, used);
            shomNumbers.remove(shomNumbers.size() - 1);
            used[i] = false;
        }
        return answer;
    }
}
~~~
