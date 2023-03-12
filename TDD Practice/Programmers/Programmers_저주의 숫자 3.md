# 저주의 숫자 3 (Level 0)

### 문제 설명

3x 마을 사람들은 3을 저주의 숫자라고 생각하기 때문에 3의 배수와 숫자 3을 사용하지 않습니다. 3x 마을 사람들의 숫자는 다음과 같습니다.

|10진법|	3x 마을에서 쓰는 숫자	|10진법|	3x 마을에서 쓰는 숫자|
|-|-|-|-|
|1|	1|	6|	8|
|2|	2|	7|	10|
|3|	4|	8|	11|
|4|	5|	9|	14|
|5|	7|	10|	16|

정수 n이 매개변수로 주어질 때, n을 3x 마을에서 사용하는 숫자로 바꿔 return하도록 solution 함수를 완성해주세요.

#### 제한사항

* 1 ≤ n ≤ 100

출처 : https://programmers.co.kr/learn/courses/30/lessons/120871

---

#### 풀이

~~~java
class Solution {
    public int solution(int n) {
        int number = 0;
        for (int i = 0; i < n; i++) {
            number = findNextNumber(number);
        }
        return number;
    }

    private int findNextNumber(int number) {
        number++;
        while (isCursedNumber(number)) {
            number++;
        }
        return number;
    }

    private boolean isCursedNumber(int number) {
        if (number % 3 == 0) {
            return true;
        }

        while (number > 0) {
            if (number % 10 == 3) {
                return true;
            }
            number /= 10;
        }
        return false;
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
    void getGradientTest() {
        int number1 = 3;
        int number2 = 1;
        int number3 = 31;
        int number4 = 131;
        int number5 = 103;
        int number6 = 100;

        assertEquals(true , isCursedNumber(number1));
        assertEquals(false , isCursedNumber(number2));
        assertEquals(true , isCursedNumber(number3));
        assertEquals(true , isCursedNumber(number4));
        assertEquals(true , isCursedNumber(number5));
        assertEquals(false , isCursedNumber(number6));
    }

    @Test
    void solutionTest() {
        int n1 = 15;
        int n2 = 40;
        int n3 = 10;

        assertEquals(25, solution(n1));
        assertEquals(76, solution(n2));
        assertEquals(16, solution(n3));
    }

    public int solution(int n) {
        int number = 0;
        for (int i = 0; i < n; i++) {
            number = findNextNumber(number);
        }
        return number;
    }

    private int findNextNumber(int number) {
        number++;
        while (isCursedNumber(number)) {
            number++;
        }
        return number;
    }

    private boolean isCursedNumber(int number) {
        if (number % 3 == 0) {
            return true;
        }

        while (number > 0) {
            if (number % 10 == 3) {
                return true;
            }
            number /= 10;
        }
        return false;
    }
}
~~~
