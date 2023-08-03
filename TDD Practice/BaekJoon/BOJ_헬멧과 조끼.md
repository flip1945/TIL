# 헬멧과 조끼 (Bronze 3)

### 문제 설명

배틀그라운드라는 게임에서는 머리와 몸을 보호하기 위해 헬멧과 조끼를 입는다. 

맵에는 다양한 헬멧과 조끼가 있으며 각각 방어력을 갖고 있다. 또한 최대 1개의 헬멧과 조끼밖에 입지 못한다. 경수는 배틀그라운드에서 승리하고 싶기 때문에 시간이 걸리더라도 최고의 헬멧과 조끼를 주워서 최대의 방어력을 얻고 싶어한다.

맵에 존재하는 조끼와 헬멧의 방어력이 주어졌을 때 경수를 도와 경수가 얻을 수 있는 방어력의 최댓값을 구해주자.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/15781/1.png" width="66px">

출처 : https://www.acmicpc.net/problem/15781

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int m = scanner.nextInt();
        List<Integer> helmets = inputArmors(scanner, n);
        List<Integer> vests = inputArmors(scanner, m);

        Armor armor = new Armor(helmets, vests);
        System.out.println(armor.getMaxDefense());
    }

    private static List<Integer> inputArmors(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());
    }
}

class Armor {

    private final List<Integer> helmets;
    private final List<Integer> vests;

    public Armor(List<Integer> helmets, List<Integer> vests) {
        this.helmets = helmets;
        this.vests = vests;
    }

    public int getMaxDefense() {
        return getMaxDefenseOfArmor(helmets) + getMaxDefenseOfArmor(vests);
    }

    private Integer getMaxDefenseOfArmor(List<Integer> armors) {
        return armors.stream()
                .max(Comparator.comparingInt(Integer::intValue))
                .orElseThrow();
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

import java.util.Comparator;
import java.util.List;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideHelmetsAndArmors")
    @DisplayName("헬멧과 조끼가 주어졌을 때 최대 방어력을 구한다.")
    void testGetMaxDefense(List<Integer> helmets, List<Integer> vests, int expected) {
        Armor sut = new Armor(helmets, vests);

        int actual = sut.getMaxDefense();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideHelmetsAndArmors() {
        return Stream.of(
                Arguments.of(List.of(1), List.of(1), 2),
                Arguments.of(List.of(2), List.of(1), 3),
                Arguments.of(List.of(2, 3), List.of(1), 4),
                Arguments.of(List.of(10, 60, 15, 20, 7), List.of(1, 2, 3, 7, 5, 1, 3), 67),
                Arguments.of(List.of(1, 1000000000), List.of(20, 18, 1000000000), 2000000000)
        );
    }
}

class Armor {

    private final List<Integer> helmets;
    private final List<Integer> vests;

    public Armor(List<Integer> helmets, List<Integer> vests) {
        this.helmets = helmets;
        this.vests = vests;
    }

    public int getMaxDefense() {
        return getMaxDefenseOfArmor(helmets) + getMaxDefenseOfArmor(vests);
    }

    private Integer getMaxDefenseOfArmor(List<Integer> armors) {
        return armors.stream()
                .max(Comparator.comparingInt(Integer::intValue))
                .orElseThrow();
    }
}
~~~
