# 귀찮아 (SIB) (Silver 5)

### 문제 설명

어떤 수열에서, 연속된 3개의 수를 보았을 때, 그 수가 단조증가 수열이거나, 단조감소 수열인 경우가 없으면 이 수열을 "지그재그 수열" 이라고 말한다.

좀 더 정확하게는, 길이 N의 수열 A가 모든 1 이상 N-2 이하의 자연수 i에 대해서, Ai ≤ Ai+1 ≤ Ai+2도 만족하지 않고, Ai ≥ Ai+1 ≥ Ai+2도 만족하지 않으면, A는 지그재그 수열이다.

길이 N의 수열 A가 주어졌을 때, A의 연속된 부분 수열 중 지그재그 수열의 최대 길이를 구하여라.

길이 M의 B가 길이 N인 A의 연속된 부분 수열이라는 것은, 어떤 i가 존재 해서, B1 = Ai, B2 = Ai+1, ..., BM = Ai+M-1 인 것을 말한다.

출처 : https://www.acmicpc.net/problem/15779

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int[] numbers = new int[n];
        for (int i = 0; i < n; i++) {
            numbers[i] = scanner.nextInt();
        }

        System.out.println(getMaxSizeOfZigzagNumbers(n, numbers));
    }

    static int getMaxSizeOfZigzagNumbers(int n, int[] numbers) {
        int maxSize = 0;
        int currentSize = 2;
        char prevState = getState(numbers[1], numbers[0]);
        char state;
        for (int i = 2; i < n; i++) {
            state = getState(numbers[i], numbers[i - 1]);
            currentSize = getCurrentSize(currentSize, prevState, state);
            maxSize = Math.max(maxSize, currentSize);
            prevState = state;
        }
        return maxSize;
    }

    static char getState(int firstNumber, int secondNumber) {
        return (firstNumber == secondNumber) ? '=' : (firstNumber > secondNumber) ? '-' : '+';
    }

    static int getCurrentSize(int currentSize, char prevState, char state) {
        if (!isZigzagNumbers(prevState, state)) {
            return 2;
        }
        return currentSize + 1;
    }

    static boolean isZigzagNumbers(char prevState, char state) {
        return (state == '+' && prevState == '-') || (state == '-' && prevState == '+');
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
    void getMaxSizeOfZigzagNumbersTest() {
        assertEquals(3, getMaxSizeOfZigzagNumbers(3, new int[]{3, 2, 3}));
        assertEquals(2, getMaxSizeOfZigzagNumbers(3, new int[]{1, 2, 3}));
        assertEquals(2, getMaxSizeOfZigzagNumbers(3, new int[]{1, 1, 3}));
        assertEquals(4, getMaxSizeOfZigzagNumbers(5, new int[]{1, 3, 4, 2, 5}));
    }

    private int getMaxSizeOfZigzagNumbers(int n, int[] numbers) {
        int maxSize = 0;
        int currentSize = 2;
        char prevState = getState(numbers[1], numbers[0]);
        char state;
        for (int i = 2; i < n; i++) {
            state = getState(numbers[i], numbers[i - 1]);
            currentSize = getCurrentSize(currentSize, prevState, state);
            maxSize = Math.max(maxSize, currentSize);
            prevState = state;
        }
        return maxSize;
    }

    private char getState(int firstNumber, int secondNumber) {
        return (firstNumber == secondNumber) ? '=' : (firstNumber > secondNumber) ? '-' : '+';
    }

    private int getCurrentSize(int currentSize, char prevState, char state) {
        if (!isZigzagNumbers(prevState, state)) {
            return 2;
        }
        return currentSize + 1;
    }

    private boolean isZigzagNumbers(char prevState, char state) {
        return (state == '+' && prevState == '-') || (state == '-' && prevState == '+');
    }
}
~~~
