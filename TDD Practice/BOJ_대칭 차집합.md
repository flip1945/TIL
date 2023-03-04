# 영재의 시험 (Silver 3)

### 문제 설명

자연수를 원소로 갖는 공집합이 아닌 두 집합 A와 B가 있다. 이때, 두 집합의 대칭 차집합의 원소의 개수를 출력하는 프로그램을 작성하시오. 두 집합 A와 B가 있을 때, (A-B)와 (B-A)의 합집합을 A와 B의 대칭 차집합이라고 한다.

예를 들어, A = { 1, 2, 4 } 이고, B = { 2, 3, 4, 5, 6 } 라고 할 때,  A-B = { 1 } 이고, B-A = { 3, 5, 6 } 이므로, 대칭 차집합의 원소의 개수는 1 + 3 = 4개이다.

출처 : https://www.acmicpc.net/problem/1269

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();
        Set<Integer> a = Arrays.stream(scanner.nextLine().split(" ")).map(Integer::parseInt).collect(Collectors.toSet());
        Set<Integer> b = Arrays.stream(scanner.nextLine().split(" ")).map(Integer::parseInt).collect(Collectors.toSet());

        System.out.println(getSymmetricDifference(a, b));
    }

    static long getSymmetricDifference(Set<Integer> a, Set<Integer> b) {
        return getSubtractionCount(a, b) + getSubtractionCount(b, a);
    }

    static long getSubtractionCount(Set<Integer> a, Set<Integer> b) {
        return a.size() - b.stream().filter(a::contains).count();
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Set;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getSubtractionCountTest() {
        assertEquals(1, getSubtractionCount(Set.of(1, 2, 4), Set.of(2, 3, 4, 5, 6)));
        assertEquals(3, getSubtractionCount(Set.of(2, 3, 4, 5, 6), Set.of(1, 2, 4)));
        assertEquals(1, getSubtractionCount(Set.of(1), Set.of(2)));
        assertEquals(0, getSubtractionCount(Set.of(1, 2), Set.of(1, 2)));
        assertEquals(0, getSubtractionCount(Set.of(1, 2), Set.of(1, 2, 3)));
        assertEquals(1, getSubtractionCount(Set.of(1, 2, 3), Set.of(1, 2)));
    }

    @Test
    void getSymmetricDifferenceTest() {
        assertEquals(2, getSymmetricDifference(Set.of(1), Set.of(2)));
        assertEquals(0, getSymmetricDifference(Set.of(1), Set.of(1)));
        assertEquals(1, getSymmetricDifference(Set.of(1, 2), Set.of(1, 2, 3)));
        assertEquals(4, getSymmetricDifference(Set.of(1, 2, 4), Set.of(2, 3, 4, 5, 6)));
    }

    private long getSymmetricDifference(Set<Integer> a, Set<Integer> b) {
        return getSubtractionCount(a, b) + getSubtractionCount(b, a);
    }

    private long getSubtractionCount(Set<Integer> a, Set<Integer> b) {
        return a.size() - b.stream().filter(a::contains).count();
    }
}
~~~
