# 수 이어가기 (Silver 5)

### 문제 설명

다음과 같은 규칙에 따라 수들을 만들려고 한다.

1. 첫 번째 수로 양의 정수가 주어진다.
2. 두 번째 수는 양의 정수 중에서 하나를 선택한다.
3. 세 번째부터 이후에 나오는 모든 수는 앞의 앞의 수에서 앞의 수를 빼서 만든다. 예를 들어, 세 번째 수는 첫 번째 수에서 두 번째 수를 뺀 것이고, 네 번째 수는 두 번째 수에서 세 번째 수를 뺀 것이다.
4. 음의 정수가 만들어지면, 이 음의 정수를 버리고 더 이상 수를 만들지 않는다.

첫 번째 수로 100이 주어질 때, 두 번째 수로 60을 선택하여 위의 규칙으로 수들을 만들면 7개의 수들 100, 60, 40, 20, 20 , 0, 20이 만들어진다. 그리고 두 번째 수로 62를 선택하여 위의 규칙으로 수들을 만들면 8개의 수들 100, 62, 38, 24, 14, 10, 4, 6이 만들어진다. 위의 예에서 알 수 있듯이, 첫 번째 수가 같더라도 두 번째 수에 따라서 만들어지는 수들의 개수가 다를 수 있다.

입력으로 첫 번째 수가 주어질 때, 이 수에서 시작하여 위의 규칙으로 만들어지는 최대 개수의 수들을 구하는 프로그램을 작성하시오. 최대 개수의 수들이 여러 개일 때, 그중 하나의 수들만 출력하면 된다.

출처 : https://www.acmicpc.net/problem/2635

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<Integer> longestNumber = findLongestNumbersUnderNumber(n);
        System.out.println(longestNumber.size());
        longestNumber.forEach(num -> System.out.print(num + " "));
    }

    private static List<Integer> findLongestNumbersUnderNumber(int number) {
        List<Integer> longestNumbers = new ArrayList<>();
        for (int i = 0; i <= number; i++) {
            List<Integer> temp = createNumbers(number, i);
            if (temp.size() > longestNumbers.size()) {
                longestNumbers = temp;
            }
        }
        return longestNumbers;
    }

    private static List<Integer> createNumbers(int firstNumber, int secondNumber) {
        List<Integer> numbers = new ArrayList<>(Arrays.asList(firstNumber, secondNumber));
        int diff = firstNumber - secondNumber;
        while (diff >= 0) {
            numbers.add(diff);
            diff = numbers.get(numbers.size() - 2) - numbers.get(numbers.size() - 1);
        }
        return numbers;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void createNumbersTest() {
        assertEquals(List.of(100, 60, 40, 20, 20, 0, 20), createNumbers(100, 60));
        assertEquals(List.of(100, 62, 38, 24, 14, 10, 4, 6), createNumbers(100, 62));
        assertEquals(List.of(10, 9, 1, 8), createNumbers(10, 9));
        assertEquals(List.of(100, 99, 1, 98), createNumbers(100, 99));
        assertEquals(List.of(10, 8, 2, 6), createNumbers(10, 8));
        assertEquals(List.of(10, 0, 10), createNumbers(10, 0));
        assertEquals(List.of(1, 1, 0, 1), createNumbers(1, 1));
    }

    @Test
    void findLongestNumbersUnderNumberTest() {
        assertEquals(List.of(100, 62, 38, 24, 14, 10, 4, 6), findLongestNumbersUnderNumber(100));
        assertEquals(List.of(10, 6, 4, 2, 2, 0, 2), findLongestNumbersUnderNumber(10));
        assertEquals(List.of(1, 1, 0, 1), findLongestNumbersUnderNumber(1));
    }

    private List<Integer> findLongestNumbersUnderNumber(int number) {
        List<Integer> longestNumbers = new ArrayList<>();
        for (int i = 0; i <= number; i++) {
            List<Integer> temp = createNumbers(number, i);
            if (temp.size() > longestNumbers.size()) {
                longestNumbers = temp;
            }
        }
        return longestNumbers;
    }

    private List<Integer> createNumbers(int firstNumber, int secondNumber) {
        List<Integer> numbers = new ArrayList<>(Arrays.asList(firstNumber, secondNumber));
        int diff = firstNumber - secondNumber;
        while (diff >= 0) {
            numbers.add(diff);
            diff = numbers.get(numbers.size() - 2) - numbers.get(numbers.size() - 1);
        }
        return numbers;
    }
}
~~~
