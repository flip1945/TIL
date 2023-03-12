# 가희와 3단 고음 (Silver 4)

### 문제 설명

I'm in my dream\~eam\~eam ♬

3단 고음에 감명을 받은 가희는 고음 경진대회를 참관하기로 했다. 음의 계이름을 수로 표현해보자. '1옥타브 도'를 1로 표현하고 1음 올라갈 때마다 그 음을 표현하는 수도 1씩 커진다고 생각할 수 있다. 음 A를 시작으로 D음씩 올리면서 고음을 부르는 경우는 첫항이 A, 공차가 D인 등차수열로 표현되며, 이러한 등차수열의 항의 개수를 X라 할 때, 이 등차수열을 X단 고음이라고 한다. 아래는 A = 1, D = 2인 6단 고음이다.

<img src="https://upload.acmicpc.net/062d4f02-7bee-4b8b-930c-da6b711c4add/-/preview/">

이러한 경진대회에는 문제가 있었는데, 한 명 이상의 참가자들이 동시에 고음을 부르는 탓에 심사를 제대로 할 수 없다는 것이다. 그래서 우리는 수로 표현된 참가자들의 음이 순서대로 주어졌을 때 가능한 경우 중, 음 A를 시작으로 D음씩 올라가는 X단 고음으로 가능한 가장 큰 X를 구하려고 한다. 이를 도와주는 프로그램을 작성하자.

출처 : https://www.acmicpc.net/problem/16162

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int a = scanner.nextInt();
        int d = scanner.nextInt();
        int[] numbers = IntStream.range(0, n).map(i -> scanner.nextInt()).toArray();

        System.out.println(getHigherOrder(numbers, a, d));
    }

    static int getHigherOrder(int[] numbers, int initialNumber, int commonDifference) {
        int currentNumber = initialNumber - commonDifference;
        for (int number : numbers) {
            currentNumber = getCurrentNumber(commonDifference, currentNumber, number);
        }
        return (currentNumber - initialNumber) / commonDifference + 1;
    }

    static int getCurrentNumber(int commonDifference, int currentNumber, int number) {
        if (isNextSequence(commonDifference, currentNumber, number)) {
            currentNumber = number;
        }
        return currentNumber;
    }

    static boolean isNextSequence(int commonDifference, int currentNumber, int number) {
        return (currentNumber + commonDifference) == number;
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
    void getHigherOrderTest() {
        assertEquals(2, getHigherOrder(new int[]{1, 3}, 1, 2));
        assertEquals(3, getHigherOrder(new int[]{1, 3, 5}, 1, 2));
        assertEquals(1, getHigherOrder(new int[]{1, 4, 5}, 1, 2));
        assertEquals(1, getHigherOrder(new int[]{3, 1, 5}, 1, 2));
        assertEquals(3, getHigherOrder(new int[]{3, 3, 9, 7, 2, 6, 9}, 3, 3));
    }

    private int getHigherOrder(int[] numbers, int initialNumber, int commonDifference) {
        int currentNumber = initialNumber - commonDifference;
        for (int number : numbers) {
            currentNumber = getCurrentNumber(commonDifference, currentNumber, number);
        }
        return (currentNumber - initialNumber) / commonDifference + 1;
    }

    private int getCurrentNumber(int commonDifference, int currentNumber, int number) {
        if (isNextSequence(commonDifference, currentNumber, number)) {
            currentNumber = number;
        }
        return currentNumber;
    }

    private boolean isNextSequence(int commonDifference, int currentNumber, int number) {
        return (currentNumber + commonDifference) == number;
    }
}
~~~
