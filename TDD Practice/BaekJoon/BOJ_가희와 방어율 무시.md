# 가희와 방어율 무시 (Bronze 4)

### 문제 설명

메이플스토리 몬스터는 방어율 수치가 있습니다. 이 방어율 수치의 일정 %를 무시하는 것을 방무라고 합니다. 유저는 아이템을 사거나, 특정한 스킬 레벨을 올려서 방무 수치를 올릴 수 있습니다. 그렇게 해서, 유저가 체감하는 몬스터의 방어율 수치를 낮출 수 있습니다. 몬스터의 방어율이 200이고, 유저의 방무가 20이라면, 몬스터의 방어율 200의 20%를 무시하게 되므로, 40만큼 무시하게 됩니다. 즉, 160이 유저가 체감하는 방어율 수치가 됩니다.

유저가 체감하는 몬스터의 방어율 수치가 100보다 크거나 같으면 몬스터에게 대미지를 줄 수 없습니다. 몬스터의 방어율 수치를 a, 유저의 방무를 b라고 할 때, 유저가 몬스터에게 대미지를 줄 수 있는지 없는지 알려주세요.  

출처 : https://www.acmicpc.net/problem/25238

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Armor armor = new Armor(scanner.nextInt(), scanner.nextInt());
        System.out.println(armor.canDamage());
    }
}

class Armor {

    private static final double PERCENT = 100.0;
    private static final int DAMAGE_THRESHOLD = 100;
    private static final int DAMAGE = 1;
    private static final int NO_DAMAGE = 0;

    private final int defense;
    private final int defenseIgnoreRate;

    public Armor(int defense, int defenseIgnoreRate) {
        this.defense = defense;
        this.defenseIgnoreRate = defenseIgnoreRate;
    }

    public int canDamage() {
        return getReducedDefense() < DAMAGE_THRESHOLD ? DAMAGE : NO_DAMAGE;
    }

    private double getReducedDefense() {
        return defense - (defense * defenseIgnoreRate / PERCENT);
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

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideDefenseAndDefenseIgnoreRate")
    @DisplayName("몬스터에게 데미지를 줄 수 있다면 1을 아니라면 0을 반환한다.")
    void testCanDamage(int defense, int defenseIgnoreRate, int expected) {
        Armor sut = new Armor(defense, defenseIgnoreRate);

        int actual = sut.canDamage();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideDefenseAndDefenseIgnoreRate() {
        return Stream.of(
                Arguments.of(200, 20, 0),
                Arguments.of(120, 20, 1),
                Arguments.of(120, 10, 0),
                Arguments.of(90, 0, 1),
                Arguments.of(336, 71, 1)
        );
    }
}

class Armor {

    private static final double PERCENT = 100.0;
    private static final int DAMAGE_THRESHOLD = 100;
    private static final int DAMAGE = 1;
    private static final int NO_DAMAGE = 0;

    private final int defense;
    private final int defenseIgnoreRate;

    public Armor(int defense, int defenseIgnoreRate) {
        this.defense = defense;
        this.defenseIgnoreRate = defenseIgnoreRate;
    }

    public int canDamage() {
        return getReducedDefense() < DAMAGE_THRESHOLD ? DAMAGE : NO_DAMAGE;
    }

    private double getReducedDefense() {
        return defense - (defense * defenseIgnoreRate / PERCENT);
    }
}
~~~
