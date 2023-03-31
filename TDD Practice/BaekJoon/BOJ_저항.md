# 저항 (Bronze 2)

### 문제 설명

전자 제품에는 저항이 들어간다. 저항은 색 3개를 이용해서 그 저항이 몇 옴인지 나타낸다. 처음 색 2개는 저항의 값이고, 마지막 색은 곱해야 하는 값이다. 저항의 값은 다음 표를 이용해서 구한다.

|색|	값|	곱|
|-|-|-|
|black	|0	|1|
|brown	|1	|10|
|red	|2	|100|
|orange	|3	|1,000|
|yellow	|4	|10,000|
|green	|5	|100,000|
|blue	|6	|1,000,000|
|violet	|7	|10,000,000|
|grey	|8	|100,000,000|
|white	|9	|1,000,000,000|

예를 들어, 저항의 색이 yellow, violet, red였다면 저항의 값은 4,700이 된다.

출처 : https://www.acmicpc.net/problem/1076

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    static Map<String, Integer> registerValues = getRegisterValues();

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(calculateRegister(scanner.nextLine(), scanner.nextLine(), scanner.nextLine()));
    }

    static long calculateRegister(String color1, String color2, String color3) {
        return (long) (getRegisterValue(color1, color2) * getRegisterExponential(color3));
    }

    static int getRegisterValue(String color1, String color2) {
        return registerValues.get(color1) * 10 + registerValues.get(color2);
    }

    static double getRegisterExponential(String color) {
        return Math.pow(10, registerValues.get(color));
    }

    static Map<String, Integer> getRegisterValues() {
        return Map.of("black", 0, "brown", 1, "red", 2, "orange", 3, "yellow", 4,
                "green", 5, "blue", 6, "violet", 7, "grey", 8, "white", 9);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Map;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {
    Map<String, Integer> registerValues = getRegisterValues();

    @Test
    void calculateRegisterTest() {
        assertEquals(0, calculateRegister("black", "black", "black"));
        assertEquals(11, calculateRegister("brown", "brown", "black"));
        assertEquals(12, calculateRegister("brown", "red", "black"));
        assertEquals(13, calculateRegister("brown", "orange", "black"));
        assertEquals(14, calculateRegister("brown", "yellow", "black"));
        assertEquals(15, calculateRegister("brown", "green", "black"));
        assertEquals(61, calculateRegister("blue", "brown", "black"));
        assertEquals(81, calculateRegister("grey", "brown", "black"));
        assertEquals(91, calculateRegister("white", "brown", "black"));
        assertEquals(9100, calculateRegister("white", "brown", "red"));
        assertEquals(4700, calculateRegister("yellow", "violet", "red"));
        assertEquals(32000000, calculateRegister("orange", "red", "blue"));
        assertEquals(99000000000L, calculateRegister("white", "white", "white"));
    }

    private long calculateRegister(String color1, String color2, String color3) {
        return (long) (getRegisterValue(color1, color2) * getRegisterExponential(color3));
    }

    private int getRegisterValue(String color1, String color2) {
        return registerValues.get(color1) * 10 + registerValues.get(color2);
    }

    private double getRegisterExponential(String color) {
        return Math.pow(10, registerValues.get(color));
    }

    private Map<String, Integer> getRegisterValues() {
            return Map.of("black", 0, "brown", 1, "red", 2, "orange", 3, "yellow", 4,
                    "green", 5, "blue", 6, "violet", 7, "grey", 8, "white", 9);
    }
}
~~~
