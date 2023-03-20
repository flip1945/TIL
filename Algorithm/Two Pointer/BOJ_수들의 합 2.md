# 수들의 합 2 (Silver 4)

### 문제

N개의 수로 된 수열 A[1], A[2], …, A[N] 이 있다. 이 수열의 i번째 수부터 j번째 수까지의 합 A[i] + A[i+1] + … + A[j-1] + A[j]가 M이 되는 경우의 수를 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 N(1 ≤ N ≤ 10,000), M(1 ≤ M ≤ 300,000,000)이 주어진다. 다음 줄에는 A[1], A[2], …, A[N]이 공백으로 분리되어 주어진다. 각각의 A[x]는 30,000을 넘지 않는 자연수이다.

---

#### 출력

첫째 줄에 경우의 수를 출력한다.

출처 : https://www.acmicpc.net/problem/2003

---

### 문제풀이

이번 문제는 투포인터 문제입니다.

어떠한 구간의 합이 m과 같은지 계속 확인하는 문제입니다.

n의 크기가 10,000이기 때문에 완전 탐색으로 문제를 풀려고 하면 시간 초과가 발생할 것입니다.

때문에 투포인터를 이용해 m보다 작은 경우 오른쪽 포인터를 증가시키고 그 외의 경우는 왼쪽 포인터를 증가시키는 방식으로 문제를 해결할 수 있습니다.

---

#### 나의 풀이

~~~java
import java.util.*;

public class Main {
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int m = scanner.nextInt();
        int[] prefixSum = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            prefixSum[i] = prefixSum[i - 1] + scanner.nextInt();
        }

        int answer = 0;
        int left = 0;
        int right = 1;
        while (right <= n) {
            int diff = prefixSum[right] - prefixSum[left];
            if (diff < m) {
                right++;
            } else {
                left++;
            }

            answer = (diff == m) ? answer + 1 : answer;
        }

        System.out.println(answer);
    }
}
~~~
