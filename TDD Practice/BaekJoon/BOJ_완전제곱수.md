# 완전제곱수 (Bronze 2)

### 문제 설명

M과 N이 주어질 때 M이상 N이하의 자연수 중 완전제곱수인 것을 모두 골라 그 합을 구하고 그 중 최솟값을 찾는 프로그램을 작성하시오. 예를 들어 M=60, N=100인 경우 60이상 100이하의 자연수 중 완전제곱수는 64, 81, 100 이렇게 총 3개가 있으므로 그 합은 245가 되고 이 중 최솟값은 64가 된다.

출처 : https://www.acmicpc.net/problem/1977

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int m = scanner.nextInt();
        int n = scanner.nextInt();
        List<Integer> perfectSquareNumbers = getPerfectSquareNumbers(m, n);
        if (perfectSquareNumbers.size() == 0) {
            System.out.println(-1);
            System.exit(0);
        }

        System.out.println(perfectSquareNumbers.stream().reduce(0, Integer::sum));
        System.out.println(perfectSquareNumbers.stream().min(Integer::compare).orElse(0));
    }

    static List<Integer> getPerfectSquareNumbers(int startNumber, int endNumber) {
        return IntStream.rangeClosed(startNumber, endNumber)
                .filter(Main::isPerfectSquareNumber)
                .boxed()
                .collect(Collectors.toList());
    }

    static boolean isPerfectSquareNumber(int i) {
        return Math.sqrt(i) % 1 == 0;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getPerfectSquareNumbersTest() {
        assertEquals(List.of(1, 4), getPerfectSquareNumbers(1, 5));
        assertEquals(List.of(1, 4, 9), getPerfectSquareNumbers(1, 10));
        assertEquals(List.of(64, 81, 100), getPerfectSquareNumbers(60, 100));
        assertEquals(List.of(), getPerfectSquareNumbers(75, 80));
        assertEquals(List.of(121), getPerfectSquareNumbers(121, 121));
    }

    private List<Integer> getPerfectSquareNumbers(int startNumber, int endNumber) {
        return IntStream.rangeClosed(startNumber, endNumber)
                .filter(this::isPerfectSquareNumber)
                .boxed()
                .collect(Collectors.toList());
    }

    private boolean isPerfectSquareNumber(int i) {
        return Math.sqrt(i) % 1 == 0;
    }
}
~~~
