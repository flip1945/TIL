# 세 수 고르기 (Silver 2)

### 문제 설명

M개의 자연수로 이루어진 집합 S와 자연수 N이 주어진다.

S에 속하지 않는 자연수 x, y, z를 골라서, |N - xyz|의 최솟값을 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1503

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int m = scanner.nextInt();
        Set<Integer> s = new HashSet<>();

        if (m != 0) {
            for (int i = 0; i < m; i++) {
                s.add(scanner.nextInt());
            }
        }

        System.out.println(getMinOfProductOfThreeNumbers(n, s));
    }

    static int getMinOfProductOfThreeNumbers(int n, Set<Integer> s) {
        int min = Integer.MAX_VALUE;
        List<Integer> selectableNumbers = getSelectableNumber(s);
        for (int selectableNumber : selectableNumbers) {
            min = getMinOfProductOfTwoNumbers(n, selectableNumbers, min, selectableNumber);
        }
        return min;
    }

    static List<Integer> getSelectableNumber(Set<Integer> s) {
        List<Integer> selectableNumbers = new ArrayList<>();
        for (int i = 1; i <= 1001; i++) {
            if (s.contains(i)) {
                continue;
            }
            selectableNumbers.add(i);
        }
        return selectableNumbers;
    }

    static int getMinOfProductOfTwoNumbers(int n, List<Integer> selectableNumbers, int min, int product) {
        for (int selectableNumber : selectableNumbers) {
            min = getMinOfProduct(n, selectableNumbers, min, product * selectableNumber);
        }
        return min;
    }

    static int getMinOfProduct(int n, List<Integer> selectableNumbers, int min, int product) {
        for (int selectableNumber : selectableNumbers) {
            min = Math.min(min, Math.abs(n - (product * selectableNumber)));
        }
        return min;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.List;
import java.util.Set;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void selectThreeNumbersTest() {
        assertEquals(1, getMinOfProductOfThreeNumbers(4, Set.of(2, 4)));
        assertEquals(2, getMinOfProductOfThreeNumbers(10, Set.of(1)));
        assertEquals(17, getMinOfProductOfThreeNumbers(10, Set.of(1, 2)));
        assertEquals(0, getMinOfProductOfThreeNumbers(10, Set.of()));
    }

    private int getMinOfProductOfThreeNumbers(int n, Set<Integer> s) {
        int min = Integer.MAX_VALUE;
        List<Integer> selectableNumbers = getSelectableNumber(s);
        for (int selectableNumber : selectableNumbers) {
            min = getMinOfProductOfTwoNumbers(n, selectableNumbers, min, selectableNumber);
        }
        return min;
    }

    private List<Integer> getSelectableNumber(Set<Integer> s) {
        List<Integer> selectableNumbers = new ArrayList<>();
        for (int i = 1; i <= 1001; i++) {
            if (s.contains(i)) {
                continue;
            }
            selectableNumbers.add(i);
        }
        return selectableNumbers;
    }

    private int getMinOfProductOfTwoNumbers(int n, List<Integer> selectableNumbers, int min, int product) {
        for (int selectableNumber : selectableNumbers) {
            min = getMinOfProduct(n, selectableNumbers, min, product * selectableNumber);
        }
        return min;
    }

    private int getMinOfProduct(int n, List<Integer> selectableNumbers, int min, int product) {
        for (int selectableNumber : selectableNumbers) {
            min = Math.min(min, Math.abs(n - (product * selectableNumber)));
        }
        return min;
    }
}
~~~
