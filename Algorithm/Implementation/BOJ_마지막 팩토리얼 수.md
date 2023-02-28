# 마지막 팩토리얼 수 (Silver 2)

### 문제

N!의 값을 계산한 후에, 0이 아닌 가장 낮은 자리 수를 구하시오.

예를 들어, 4! = 24 이기 때문에, 0이 아닌 가장 낮은 자리 수는 4이다. 또, 5! = 120이기 때문에, 0이 아닌 가장 낮은 자리 수는 2 이다.

---

#### 입력

첫째 줄에 N이 주어진다. N은 20,000보다 작거나 같은 자연수 이다.

---

#### 출력

첫째 줄에 N!의 0이 아닌 마지막 자리수를 출력한다.

출처 : https://www.acmicpc.net/problem/2553

---

### 문제풀이

이번 문제는 구현 문제입니다.

팩토리얼의 가장 낮은 자리의 숫자를 구해야 하는데, 0인 경우는 그 다음으로 낮은 자리의 숫자를 구해야합니다.

처음에는 일의 자리 숫자만 남겨서 계산하는 방식으로 문제를 풀었는데 일의 자리 숫자만으로는 정보가 부족해서 원하는 답을 만들어내지 못 했습니다.

범위를 넓혀서 100의 자리까지 해봤는데 여전히 틀려서 값을 점차 넓혀봤습니다.

결국 1000000자리 이하의 숫자를 남겼을 때 문제를 해결할 수 있었습니다.

---

#### 나의 풀이

~~~python
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();

        long factorial = 1;
        for (int i = 1; i <= n; i++) {
            factorial *= i;
            while (factorial % 10 == 0) {
                factorial /= 10;
            }
            factorial %= 100000;
        }
        System.out.println(factorial % 10);
    }
}
~~~
