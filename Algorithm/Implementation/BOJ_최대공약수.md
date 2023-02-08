# 소수인팰린드롬 (Gold 5)

### 문제

상근이는 학생들에게 두 양의 정수 A와 B의 최대공약수를 계산하는 문제를 내주었다. 그런데, 상근이는 학생들을 골탕먹이기 위해 매우 큰 A와 B를 주었다.

상근이는 N개의 수와 M개의 수를 주었고, N개의 수를 모두 곱하면 A, M개의 수를 모두 곱하면 B가 된다.

이 수가 주어졌을 때, 최대공약수를 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 N(1 ≤ N ≤ 1000)이 주어진다. 둘째 줄에는 N개의 양의 정수가 공백으로 구분되어 주어진다. 이 수는 모두 1,000,000,000보다 작고, N개의 수를 곱하면 A가 된다.

셋째 줄에 M(1 ≤ M ≤ 1000)이 주어진다. 넷째 줄에는 M개의 양의 정수가 공백으로 구분되어 주어진다. 이 수는 모두 1,000,000,000보다 작고, M개의 수를 곱하면 B가 된다.

---

#### 출력

두 수의 최대공약수를 출력한다. 만약, 9자리보다 길다면, 마지막 9자리만 출력한다. (최대 공약수가 1000012028인 경우에는 000012028을 출력해야 한다)

출처 : https://www.acmicpc.net/problem/2824

---

### 문제풀이

이번 문제는 아주 큰 수의 최대공약수를 구하는 문제입니다.

원래 문제의 의도와 맞게 풀었는지 궁금하지만 저는 Java의 BigInteger를 이용해 문제를 해결했습니다.

이전에 Java에서 아주 큰 수를 다루기 위해 BigInteger를 써본 적이 있어서 쉽게 풀 수 있었습니다.

---

#### 나의 풀이

~~~python
import java.math.*;
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        BigInteger a = new BigInteger("1");
        for (int i = 0; i < n; i++) {
            a = a.multiply(BigInteger.valueOf(scanner.nextLong()));
        }
        int m = scanner.nextInt();
        BigInteger b = new BigInteger("1");
        for (int i = 0; i < m; i++) {
            b = b.multiply(BigInteger.valueOf(scanner.nextLong()));
        }

        String answer = a.gcd(b).toString();
        if (answer.length() > 9) {
            System.out.println(answer.substring(answer.length() - 9));
            return;
        }
        System.out.println(answer);
    }
}
~~~
