# 박 터뜨리기 (Silver 4)

### 문제 설명

$K$개의 팀이 박 터트리기 게임을 한다. 각 팀은 하나의 바구니를 가지고 있고, 바구니에 들어있는 공을 던져서 자기 팀의 박을 터트려야 한다.

우리는 게임을 준비하기 위해서, $N$개의 공을 $K$개의 바구니에 나눠 담아야 한다. 이때, 게임의 재미를 위해서 바구니에 담기는 공의 개수를 모두 다르게 하고 싶다. 즉, $N$개의 공을 $K$개의 바구니에 빠짐없이 나누어 담는데, 각 바구니에는 1개 이상의 공이 있어야 하고, 바구니에 담긴 공의 개수가 모두 달라야 한다.

게임의 불공정함을 줄이기 위해서, 가장 많이 담긴 바구니와 가장 적게 담긴 바구니의 공의 개수 차이가 최소가 되도록 담을 것이다.

공을 바구니에 나눠 담기 위한 규칙을 정리하면 다음과 같다.

1. $N$개의 공을 $K$개의 바구니에 빠짐없이 나누어 담는다.
2. 각 바구니에는 1개 이상의 공이 들어 있어야 한다.
3. 각 바구니에 담긴 공의 개수는 모두 달라야 한다.
4. 가장 많이 담긴 바구니와 가장 적게 담긴 바구니의 공의 개수 차이가 최소가 되어야 한다.

위의 규칙을 모두 만족하며 $N$개의 공을 $K$개의 바구니에 나눠 담을 때, 나눠 담을 수 있는지 여부를 결정하고, 담을 수 있는 경우에는 가장 많이 담긴 바구니와 가장 적게 담긴 바구니의 공의 개수 차이를 계산해서 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/19939

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int k = scanner.nextInt();

        System.out.println(solution(n, k));
    }

    private static int solution(int n, int k) {
        if (isUnderGaussSum(n, k)) {
            return -1;
        }
        return getMinBallDiff(n, k);
    }

    private static boolean isUnderGaussSum(int n, int k) {
        return n < getGaussSum(k);
    }

    private static int getGaussSum(int k) {
        return k * (k + 1) / 2;
    }

    private static int getMinBallDiff(int n, int k) {
        int gaussSum = getGaussSum(k);
        int remainder = (n - gaussSum) % k;
        return (remainder == 0) ? k - 1 : k;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void solutionTest() {
        assertEquals(-1, solution(5, 3));
        assertEquals(2, solution(6, 3));
        assertEquals(3, solution(7, 3));
        assertEquals(3, solution(8, 3));
        assertEquals(2, solution(9, 3));
        assertEquals(3, solution(10, 3));
        assertEquals(3, solution(11, 3));

        assertEquals(-1, solution(9, 4));
        assertEquals(3, solution(10, 4));
        assertEquals(4, solution(11, 4));

        assertEquals(-1, solution(20, 6));
        assertEquals(5, solution(21, 6));
        assertEquals(6, solution(22, 6));
        assertEquals(6, solution(23, 6));
        assertEquals(6, solution(24, 6));
        assertEquals(6, solution(25, 6));
        assertEquals(6, solution(26, 6));
        assertEquals(5, solution(27, 6));

        assertEquals(99, solution(5050, 100));
    }

    private int solution(int n, int k) {
        if (isUnderGaussSum(n, k)) {
            return -1;
        }
        return getMinBallDiff(n, k);
    }

    private boolean isUnderGaussSum(int n, int k) {
        return n < getGaussSum(k);
    }

    private int getGaussSum(int k) {
        return k * (k + 1) / 2;
    }

    private int getMinBallDiff(int n, int k) {
        int gaussSum = getGaussSum(k);
        int remainder = (n - gaussSum) % k;
        return (remainder == 0) ? k - 1 : k;
    }
}
~~~
