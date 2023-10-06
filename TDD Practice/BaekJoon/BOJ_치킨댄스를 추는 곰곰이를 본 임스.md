# 치킨댄스를 추는 곰곰이를 본 임스 (Bronze 4)

### 문제 설명

치킨 댄스를 추고 있는 곰곰이를 본 임스는 치킨을 먹고 싶어졌다. 임스는 치킨 $1$마리를 먹을 때 반드시 집에 있는 콜라 $2$개나 맥주 $1$개와 같이 먹어야 한다.

또한, 치킨집에 있는 치킨의 개수보다 치킨을 많이 시켜먹을 수는 없다.

치킨집에 있는 치킨의 개수와 임스의 집에 있는 콜라, 맥주의 개수가 주어졌을 때, 임스가 시켜먹을 수 있는 치킨의 총 개수를 출력하시오.

출처 : https://www.acmicpc.net/problem/25191

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Chicken chicken = new Chicken(scanner.nextInt(), scanner.nextInt(), scanner.nextInt());
        System.out.println(chicken.count());
    }
}

class Chicken {

    private static final int COLA_PER_CHICKEN = 2;

    private final int countOfChickenInStore;
    private final int countOfCola;
    private final int countOfBeer;

    public Chicken(int countOfChickenInStore, int countOfCola, int countOfBeer) {
        this.countOfChickenInStore = countOfChickenInStore;
        this.countOfCola = countOfCola;
        this.countOfBeer = countOfBeer;
    }

    public int count() {
        return countOfChickenWithStore();
    }

    private int countOfChickenWithStore() {
        return Math.min(eatableChickenCount(), countOfChickenInStore);
    }

    private int eatableChickenCount() {
        return eatableChickenCountOfCola() + eatableChickenCountOfBeer();
    }

    private int eatableChickenCountOfCola() {
        return countOfCola / COLA_PER_CHICKEN;
    }

    private int eatableChickenCountOfBeer() {
        return countOfBeer;
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

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideChickenAndColaAndBeer")
    @DisplayName("시켜먹을 수 있는 치킨의 총 개수를 반환한다.")
    void testChicken(int chicken, int cola, int beer, int expected) {
        Chicken sut = new Chicken(chicken, cola, beer);

        int actual = sut.count();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideChickenAndColaAndBeer() {
        return Stream.of(
                Arguments.of(1, 1, 1, 1),
                Arguments.of(2, 2, 1, 2),
                Arguments.of(3, 2, 1, 2),
                Arguments.of(5, 4, 2, 4),
                Arguments.of(3, 4, 2, 3)
        );
    }
}

class Chicken {

    private static final int COLA_PER_CHICKEN = 2;

    private final int countOfChickenInStore;
    private final int countOfCola;
    private final int countOfBeer;

    public Chicken(int countOfChickenInStore, int countOfCola, int countOfBeer) {
        this.countOfChickenInStore = countOfChickenInStore;
        this.countOfCola = countOfCola;
        this.countOfBeer = countOfBeer;
    }

    public int count() {
        return countOfChickenWithStore();
    }

    private int countOfChickenWithStore() {
        return Math.min(eatableChickenCount(), countOfChickenInStore);
    }

    private int eatableChickenCount() {
        return eatableChickenCountOfCola() + eatableChickenCountOfBeer();
    }

    private int eatableChickenCountOfCola() {
        return countOfCola / COLA_PER_CHICKEN;
    }

    private int eatableChickenCountOfBeer() {
        return countOfBeer;
    }
}
~~~
