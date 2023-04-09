# 화성 수학 (Bronze 2)

### 문제 설명

겨울 방학에 달에 다녀온 상근이는 여름 방학 때는 화성에 갔다 올 예정이다. (3996번) 화성에서는 지구와는 조금 다른 연산자 @, %, #을 사용한다. @는 3을 곱하고, %는 5를 더하며, #는 7을 빼는 연산자이다. 따라서, 화성에서는 수학 식의 가장 앞에 수가 하나 있고, 그 다음에는 연산자가 있다.

출처 : https://www.acmicpc.net/problem/5430

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = Integer.parseInt(scanner.nextLine());
        while (t --> 0) {
            List<String> marsMaths = List.of(scanner.nextLine().split(" "));
            System.out.println(calcMarsMath(marsMaths));
        }
    }

    static String calcMarsMath(List<String> marsMaths) {
        double result = calcMarsMathWithSigns(marsMaths.subList(1, marsMaths.size()), Double.parseDouble(marsMaths.get(0)));
        return resultToString(result);
    }

    static double calcMarsMathWithSigns(List<String> marsMaths, double result) {
        for (String sign : marsMaths) {
            result = calcMarsMathWithSign(result, sign);
        }
        return result;
    }

    static double calcMarsMathWithSign(double result, String sign) {
        switch (sign) {
            case "@": return result * 3;
            case "%": return result + 5;
            case "#": return result - 7;
            default: throw new IllegalStateException("Unexpected value: " + sign);
        }
    }

    static String resultToString(double result) {
        return String.format("%.2f", result);
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
    void marsMathTest() {
        assertEquals("0.00", calcMarsMath(List.of("0", "@")));
        assertEquals("3.00", calcMarsMath(List.of("1", "@")));
        assertEquals("6.00", calcMarsMath(List.of("2", "@")));
        assertEquals("9.00", calcMarsMath(List.of("3", "@")));
        assertEquals("4.50", calcMarsMath(List.of("1.5", "@")));
        assertEquals("6.00", calcMarsMath(List.of("1", "%")));
        assertEquals("6.50", calcMarsMath(List.of("1.5", "%")));
        assertEquals("19.50", calcMarsMath(List.of("1.5", "%", "@")));
        assertEquals("-5.50", calcMarsMath(List.of("1.5", "#")));
        assertEquals("14.00", calcMarsMath(List.of("3", "@", "%")));
        assertEquals("25.20", calcMarsMath(List.of("10.4", "#", "%", "@")));
        assertEquals("1.00", calcMarsMath(List.of("8", "#")));
    }

    private String calcMarsMath(List<String> marsMaths) {
        double result = calcMarsMathWithSigns(marsMaths.subList(1, marsMaths.size()), Double.parseDouble(marsMaths.get(0)));
        return resultToString(result);
    }

    private double calcMarsMathWithSigns(List<String> marsMaths, double result) {
        for (String sign : marsMaths) {
            result = calcMarsMathWithSign(result, sign);
        }
        return result;
    }

    private double calcMarsMathWithSign(double result, String sign) {
        switch (sign) {
            case "@": return result * 3;
            case "%": return result + 5;
            case "#": return result - 7;
            default: throw new IllegalStateException("Unexpected value: " + sign);
        }
    }

    private String resultToString(double result) {
        return String.format("%.2f", result);
    }
}
~~~
