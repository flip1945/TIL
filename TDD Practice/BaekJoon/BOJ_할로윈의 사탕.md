# 할로윈의 사탕 (Bronze 3)

### 문제 설명

할로윈데이에 한신이네는 아부지가 사탕을 나눠주신다. 하지만 한신이의 형제들은 서로 사이가 좋지않아 서른이 넘어서도 사탕을 공정하게 나누어 주지 않으면 서로 싸움이 난다. 매년 할로윈데이때마다 아부지는 사탕을 자식들에게 최대한 많은 사탕을 나누어 주시기 원하며 자신에게는 몇개가 남게되는지에 알고 싶어 하신다. 이런 아부지를 도와서 형제간의 싸움을 막아보자.

출처 : https://www.acmicpc.net/problem/10178

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            Candies candies = new Candies(scanner.nextInt(), scanner.nextInt());
            System.out.println(candies.divideCandies());
        }
    }
}

class Candies {

    public static final String DIVIDE_RESULT_STRING = "You get %d piece(s) and your dad gets %d piece(s).";

    private final int totalCountOfCandies;
    private final int countOfPeople;

    public Candies(int totalCountOfCandies, int countOfPeople) {
        this.totalCountOfCandies = totalCountOfCandies;
        this.countOfPeople = countOfPeople;
    }

    public String divideCandies() {
        return String.format(DIVIDE_RESULT_STRING, getCountOfCandies(), getRemainCandies());
    }

    private int getCountOfCandies() {
        return totalCountOfCandies / countOfPeople;
    }

    private int getRemainCandies() {
        return totalCountOfCandies % countOfPeople;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideCountOfCandiesAndCountOfPeople")
    @DisplayName("전체 사탕의 수와 나눠야 할 사람의 수가 주어졌을 때 사탕을 나눈 결과를 반환한다.")
    void testDivideCandies(int totalCountOfCandies, int countOfPeople, String expected) {
        Candies sut = new Candies(totalCountOfCandies, countOfPeople);

        String actual = sut.divideCandies();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCountOfCandiesAndCountOfPeople() {
        return Stream.of(
                Arguments.of(1, 1, "You get 1 piece(s) and your dad gets 0 piece(s)."),
                Arguments.of(2, 1, "You get 2 piece(s) and your dad gets 0 piece(s)."),
                Arguments.of(3, 2, "You get 1 piece(s) and your dad gets 1 piece(s)."),
                Arguments.of(22, 3, "You get 7 piece(s) and your dad gets 1 piece(s)."),
                Arguments.of(15, 5, "You get 3 piece(s) and your dad gets 0 piece(s)."),
                Arguments.of(99, 8, "You get 12 piece(s) and your dad gets 3 piece(s)."),
                Arguments.of(7, 4, "You get 1 piece(s) and your dad gets 3 piece(s)."),
                Arguments.of(101, 5, "You get 20 piece(s) and your dad gets 1 piece(s).")
        );
    }
}

class Candies {

    public static final String DIVIDE_RESULT_STRING = "You get %d piece(s) and your dad gets %d piece(s).";

    private final int totalCountOfCandies;
    private final int countOfPeople;

    public Candies(int totalCountOfCandies, int countOfPeople) {
        this.totalCountOfCandies = totalCountOfCandies;
        this.countOfPeople = countOfPeople;
    }

    public String divideCandies() {
        return String.format(DIVIDE_RESULT_STRING, getCountOfCandies(), getRemainCandies());
    }

    private int getCountOfCandies() {
        return totalCountOfCandies / countOfPeople;
    }

    private int getRemainCandies() {
        return totalCountOfCandies % countOfPeople;
    }
}
~~~
