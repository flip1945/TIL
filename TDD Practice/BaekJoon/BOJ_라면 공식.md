# 라면 공식 (Bronze 5)

### 문제 설명

”꼬불꼬불 꼬불꼬불 맛좋은 라면 라면이 있기에 세상 살맛나 하루에 열개라도 먹을 수 있어 후루룩 짭짭 후루룩 짭짭 맛좋은 라면”

예찬이는 라면을 매우 좋아한다. 선린 최고의 라면 애호가답게, 예찬이는 한 끼에도 라면 여러 개를 흡입하고는 한다.

평소 라면을 가장 맛있게 끓일 수 있는 물의 양이 궁금했던 예찬이는 오랜 실험 끝에 마침내 아래와 같은 라면 공식을 만드는 데 성공했다.

$$W_i=A_i(X_i - 1)+B_i$$ 

단, $W_i$는 필요한 물의 양, $A_i$는 라면 계수, $B_i$는 기본 물의 양, $X_i$는 끓일 라면 수를 나타낸다.

예찬이가 라면을 끓이는 횟수 $N$과 $i$ $(1 \leq i \leq N)$번째로 라면을 끓일 때의 라면 계수 $A_i$, 기본 물의 양 $B_i$, 끓일 라면 수 $X_i$가 주어질 때, 예찬이를 위해 라면 공식에 따라 필요한 물의 양 $W_i$을 계산해 보자.

출처 : https://www.acmicpc.net/problem/30007

---

#### 풀이
~~~java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            Ramen ramen = Ramen.of(scanner.nextInt(), scanner.nextInt());
            System.out.println(ramen.getRequiredWater(scanner.nextInt()));
        }
    }
}

class Ramen {

    private final int ramenFactor;
    private final int baseWater;

    private Ramen(int ramenFactor, int baseWater) {
        this.ramenFactor = ramenFactor;
        this.baseWater = baseWater;
    }

    public static Ramen of(int ramenFactor, int baseWater) {
        return new Ramen(ramenFactor, baseWater);
    }

    public int getRequiredWater(int countOfRamen) {
        return ramenFactor * (countOfRamen - 1) + baseWater;
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
    @MethodSource("provideRamenInfo")
    @DisplayName("라면은 필요한 물의 양을 계산한다.")
    void aRamenCalculatesRequiredWater(int ramenFactor, int baseWater, int countOfRamen, int expected) {
        Ramen sut = Ramen.of(ramenFactor, baseWater);

        int actual = sut.getRequiredWater(countOfRamen);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideRamenInfo() {
        return Stream.of(
                Arguments.of(500, 550, 2, 1050),
                Arguments.of(500, 550, 1, 550),
                Arguments.of(500, 550, 3, 1550),
                Arguments.of(500, 550, 4, 2050),
                Arguments.of(450, 500, 7, 3200)
        );
    }
}

class Ramen {

    private final int ramenFactor;
    private final int baseWater;

    private Ramen(int ramenFactor, int baseWater) {
        this.ramenFactor = ramenFactor;
        this.baseWater = baseWater;
    }

    public static Ramen of(int ramenFactor, int baseWater) {
        return new Ramen(ramenFactor, baseWater);
    }

    public int getRequiredWater(int countOfRamen) {
        return ramenFactor * (countOfRamen - 1) + baseWater;
    }
}
~~~
