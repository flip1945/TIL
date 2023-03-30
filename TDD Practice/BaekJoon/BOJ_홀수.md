# 홀수 (Bronze 3)

### 문제 설명

7개의 자연수가 주어질 때, 이들 중 홀수인 자연수들을 모두 골라 그 합을 구하고, 고른 홀수들 중 최솟값을 찾는 프로그램을 작성하시오.

예를 들어, 7개의 자연수 12, 77, 38, 41, 53, 92, 85가 주어지면 이들 중 홀수는 77, 41, 53, 85이므로 그 합은

77 + 41 + 53 + 85 = 256

이 되고,

41 < 53 < 77 < 85

이므로 홀수들 중 최솟값은 41이 된다.

출처 : https://www.acmicpc.net/problem/2576

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Integer> numbers = IntStream.range(0, 7)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());

        List<Integer> odds = getOdds(numbers);
        printAnswer(odds);
    }

    static void printAnswer(List<Integer> odds) {
        if (odds.isEmpty()) {
            System.out.println(-1);
            return;
        }
        System.out.println(sumOfOdds(odds));
        System.out.println(getMinOdd(odds));
    }

    static List<Integer> getOdds(List<Integer> numbers) {
        return numbers.stream()
                .filter(n -> n % 2 == 1)
                .collect(Collectors.toList());
    }

    static int sumOfOdds(List<Integer> odds) {
        return odds.stream()
                .reduce(Integer::sum)
                .orElse(0);
    }

    static int getMinOdd(List<Integer> odds) {
        return odds.stream()
                .reduce(Integer::min)
                .orElse(0);
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
    void getOddsTest() {
        assertEquals(List.of(1, 3), getOdds(List.of(1, 2, 3)));
        assertEquals(List.of(1, 3, 5), getOdds(List.of(1, 2, 3, 5)));
        assertEquals(List.of(), getOdds(List.of(2, 2, 6, 8)));
        assertEquals(List.of(77), getOdds(List.of(77, 38)));
        assertEquals(List.of(77, 41, 53, 85), getOdds(List.of(12, 77, 38, 41, 53, 92, 85)));
    }

    @Test
    void sumOfOddsTest() {
        assertEquals(4, sumOfOdds(getOdds(List.of(1, 2, 3))));
        assertEquals(9, sumOfOdds(getOdds(List.of(1, 2, 3, 5))));
        assertEquals(0, sumOfOdds(getOdds(List.of(2, 2, 6, 8))));
        assertEquals(77, sumOfOdds(getOdds(List.of(77, 38))));
        assertEquals(256, sumOfOdds(getOdds(List.of(12, 77, 38, 41, 53, 92, 85))));
    }

    @Test
    void getMinOddTest() {
        assertEquals(1, getMinOdd(getOdds(List.of(1, 2, 3))));
        assertEquals(1, getMinOdd(getOdds(List.of(1, 2, 3, 5))));
        assertEquals(77, getMinOdd(getOdds(List.of(77, 38))));
        assertEquals(41, getMinOdd(getOdds(List.of(12, 77, 38, 41, 53, 92, 85))));
        assertEquals(0, getMinOdd(getOdds(List.of(12, 88, 100))));
    }

    private List<Integer> getOdds(List<Integer> numbers) {
        return numbers.stream()
                .filter(n -> n % 2 == 1)
                .collect(Collectors.toList());
    }

    private int sumOfOdds(List<Integer> odds) {
        return odds.stream()
                .reduce(Integer::sum)
                .orElse(0);
    }

    private int getMinOdd(List<Integer> odds) {
        return odds.stream()
                .reduce(Integer::min)
                .orElse(0);
    }
}
~~~
