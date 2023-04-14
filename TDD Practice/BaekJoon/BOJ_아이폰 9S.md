# 아이폰 9S (Silver 4)

### 문제 설명
오늘은 애플의 아이폰 9S가 출시되는 날이다. N(1 ≤ N ≤ 1000)명의 사람들은 새 아이폰을 누구보다 먼저 구매하기 위해서 애플 스토어 앞에 한 줄로 서있다. 아이폰 9S는 구매자가 용량을 마음대로 정할 수 있다. 즉, 지금까지 아이폰은 16/32/64GB와 같이 용량의 크기가 미리 정해져 있었다. 하지만, 9S는 자신이 원하는 용량의 크기 B(i)를 점원에게 말하면, 정확하게 그 용량을 가진 아이폰 9S를 그 자리에서 만들어 구매하는 방식이다.

애플 스토어의 점원 상근이는 같은 용량을 원하는 사람들이 연속되어 있으면, 더 보기 좋을 것이라고 생각한다. 따라서, 특정 사람을 고르고, 그 사람이 원하는 용량을 원하는 사람을 줄에서 모두 빼버리려고 한다.

어떤 용량을 원하는 사람을 줄에서 빼 버리면, 같은 용량을 원하는 사람들이 연속되어 있는 구간의 길이중 가장 긴 값이 최대가 되는지 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/5883

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<Integer> numbers = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());

        System.out.println(getLongestStreakWithoutOneNumber(numbers));
    }

    static int getLongestStreakWithoutOneNumber(List<Integer> numbers) {
        int result = 0;
        for (int excludedNumber : getExcludedNumber(numbers)) {
            result = Math.max(result, getStreak(numbers, excludedNumber));
        }
        return result;
    }

    static Set<Integer> getExcludedNumber(List<Integer> numbers) {
        return new HashSet<>(numbers);
    }

    static int getStreak(List<Integer> numbers, int excludedNumber) {
        int result = 0;
        int prev = -1;
        int streak = 1;
        for (int number : numbers) {
            if (number != excludedNumber) {
                streak = increaseStreakOrNot(prev, streak, number);
                result = Math.max(result, streak);
                prev = number;
            }
        }
        return result;
    }

    static int increaseStreakOrNot(int prev, int streak, int number) {
        if (number == prev) {
            return streak + 1;
        }
        return 1;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.HashSet;
import java.util.List;
import java.util.Set;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getLongestStreakWithoutOneNumberTest() {
        assertEquals(2, getLongestStreakWithoutOneNumber(List.of(1, 2, 1, 3)));
        assertEquals(3, getLongestStreakWithoutOneNumber(List.of(1, 2, 1, 1, 3)));
        assertEquals(4, getLongestStreakWithoutOneNumber(List.of(1, 2, 1, 1, 1, 3)));
        assertEquals(4, getLongestStreakWithoutOneNumber(List.of(1, 3, 1, 1, 1, 3)));
        assertEquals(4, getLongestStreakWithoutOneNumber(List.of(2, 7, 3, 7, 7, 3, 7, 5, 7)));
        assertEquals(1, getLongestStreakWithoutOneNumber(List.of(2, 1)));
        assertEquals(2, getLongestStreakWithoutOneNumber(List.of(2, 2, 1)));
        assertEquals(3, getLongestStreakWithoutOneNumber(List.of(2, 0, 0, 0)));
        assertEquals(0, getLongestStreakWithoutOneNumber(List.of(0)));
        assertEquals(1, getLongestStreakWithoutOneNumber(List.of(1, 2, 3, 4, 5)));
        assertEquals(2, getLongestStreakWithoutOneNumber(List.of(0, 1, 1, 2, 2)));
    }

    private int getLongestStreakWithoutOneNumber(List<Integer> numbers) {
        int result = 0;
        for (int excludedNumber : getExcludedNumber(numbers)) {
            result = Math.max(result, getStreak(numbers, excludedNumber));
        }
        return result;
    }

    private Set<Integer> getExcludedNumber(List<Integer> numbers) {
        return new HashSet<>(numbers);
    }

    private int getStreak(List<Integer> numbers, int excludedNumber) {
        int result = 0;
        int prev = -1;
        int streak = 1;
        for (int number : numbers) {
            if (number != excludedNumber) {
                streak = increaseStreakOrNot(prev, streak, number);
                result = Math.max(result, streak);
                prev = number;
            }
        }
        return result;
    }

    private int increaseStreakOrNot(int prev, int streak, int number) {
        if (number == prev) {
            return streak + 1;
        }
        return 1;
    }
}
~~~
