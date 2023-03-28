# 5와 6의 차이 (Bronze 2)

### 문제 설명

상근이는 2863번에서 표를 너무 열심히 돌린 나머지 5와 6을 헷갈리기 시작했다.

상근이가 숫자 5를 볼 때, 5로 볼 때도 있지만, 6으로 잘못 볼 수도 있고, 6을 볼 때는, 6으로 볼 때도 있지만, 5로 잘못 볼 수도 있다.

두 수 A와 B가 주어졌을 때, 상근이는 이 두 수를 더하려고 한다. 이때, 상근이가 구할 수 있는 두 수의 가능한 합 중, 최솟값과 최댓값을 구해 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/2864

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        String num1 = st.nextToken();
        String num2 = st.nextToken();

        solution(num1, num2).forEach(n -> System.out.print(n + " "));
    }

    static List<Integer> solution(String num1, String num2) {
        int min = getMinOfSum(num1, num2);
        int max = getMaxOfSum(num1, num2);
        return List.of(min, max);
    }

    static int getMaxOfSum(String num1, String num2) {
        int number1 = getReplacedNumber(num1, "5", "6");
        int number2 = getReplacedNumber(num2, "5", "6");
        return number1 + number2;
    }

    static int getMinOfSum(String num1, String num2) {
        int number1 = getReplacedNumber(num1, "6", "5");
        int number2 = getReplacedNumber(num2, "6", "5");
        return number1 + number2;
    }

    static int getReplacedNumber(String number, String regex, String replacement) {
        return Integer.parseInt(number.replaceAll(regex, replacement));
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void solutionTest() {
        assertEquals(List.of(33, 33), solution("11", "22"));
        assertEquals(List.of(36, 37), solution("11", "25"));
        assertEquals(List.of(6282, 6292), solution("1430", "4862"));
        assertEquals(List.of(74580, 85582), solution("16796", "58786"));
        assertEquals(List.of(2, 2), solution("1", "1"));
    }

    private List<Integer> solution(String num1, String num2) {
        int min = getMinOfSum(num1, num2);
        int max = getMaxOfSum(num1, num2);
        return List.of(min, max);
    }

    private int getMaxOfSum(String num1, String num2) {
        int number1 = getReplacedNumber(num1, "5", "6");
        int number2 = getReplacedNumber(num2, "5", "6");
        return number1 + number2;
    }

    private int getMinOfSum(String num1, String num2) {
        int number1 = getReplacedNumber(num1, "6", "5");
        int number2 = getReplacedNumber(num2, "6", "5");
        return number1 + number2;
    }

    private int getReplacedNumber(String number, String regex, String replacement) {
        return Integer.parseInt(number.replaceAll(regex, replacement));
    }
}
~~~
