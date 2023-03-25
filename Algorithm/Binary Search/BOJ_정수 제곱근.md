# 정수 제곱근 (Silver 4)

### 문제

정수가 주어지면, 그 수의 정수 제곱근을 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 정수 n이 주어진다. (0 ≤ n < 2^63)

---

#### 출력

첫째 줄에 q^2 ≥ n인 가장 작은 음이 아닌 정수 q를 출력한다.

출처 : https://www.acmicpc.net/problem/2417

---

### 문제풀이

이번 문제는 이분 탐색 문제입니다.

최대 조건이 long 범위와 같은 범위이기 때문에 long 타입을 이용해야 문제를 풀 수 있습니다.

이 문제를 풀 때 주의해야 할 점은 end의 값을 설정할 때 n이 아닌 제곱근의 최대값을 설정해야 한다는 점이고 반환값을 잘 설정해야 한다는 점입니다.

---

#### 나의 풀이

~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        long n = scanner.nextLong();

        System.out.println(binarySearch(n));
    }

    private static long binarySearch(long n) {
        long start = 0;
        long end = 3037000499L;
        while (start <= end) {
            long mid = (end - start) / 2 + start;
            long square = mid * mid;
            if (square == n) {
                return mid;
            } else if (square < n) {
                start = mid + 1;
            } else {
                end = mid - 1;
            }
        }
        return start;
    }
}
~~~
