# 호반우가 학교에 지각한 이유 1 (Bronze 4)

### 문제 설명

경북대학교의 마스코트이자 따봉 요정인 호반우는 오늘도 수업을 듣기 위해 경북대학교 정문을 지나치던 도중 정체불명의 차원문에 휘말려 이세계로 오게 되었다.

이세계의 신인 당신이 호반우가 마왕을 물리치고 지구로 돌아가 학교에 지각하지 않도록 도와주자!

호반우의 현재 스탯인 힘($STR$), 민첩($DEX$), 지능($INT$), 운($LUK$)에 해당하는 $4$가지 수가 주어진다.

$4$가지 스탯 중 하나의 수치를 $1$씩 올릴 수 있는 축복을 여러 번 사용해 호반우의 평균 스탯 수치를 $N$ 이상으로 만들려고 할 때 최소 몇 번의 축복을 사용해야 하는지 구해보자.

출처 : https://www.acmicpc.net/problem/30468

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Stats stats = new Stats(scanner.nextInt(), scanner.nextInt(), scanner.nextInt(), scanner.nextInt(), scanner.nextInt());

        System.out.println(stats.getRequiredBlessingCount());
    }
}

class Stats {

    private static final int MINIMUM_BLESSING_COUNT = 0;
    private static final int STATS_SIZE = 4;

    private final int str;
    private final int dex;
    private final int wiz;
    private final int luck;
    private final int targetAverageStat;

    public Stats(int str, int dex, int wiz, int luck, int targetAverageStat) {
        this.str = str;
        this.dex = dex;
        this.wiz = wiz;
        this.luck = luck;
        this.targetAverageStat = targetAverageStat;
    }

    public int getRequiredBlessingCount() {
        return Math.max(requiredBlessingCount(), MINIMUM_BLESSING_COUNT);
    }

    private int requiredBlessingCount() {
        return totalTargetStatCount() - sumStatsCount();
    }

    private int totalTargetStatCount() {
        return targetAverageStat * STATS_SIZE;
    }

    private int sumStatsCount() {
        return str + dex + wiz + luck;
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
    @MethodSource("provideStats")
    @DisplayName("스탯들이 주어지면 필요한 축복의 횟수를 구한다.")
    void statsReturnsRequiredBlessingCount(int str, int dex, int wiz, int luck, int targetAverageStat, int expected) {
        Stats sut = new Stats(str, dex, wiz, luck, targetAverageStat);

        int actual = sut.getRequiredBlessingCount();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideStats() {
        return Stream.of(
                Arguments.of(1, 2, 3, 4, 4, 6),
                Arguments.of(4, 5, 1, 2, 4, 4),
                Arguments.of(7, 12, 4, 23, 11, 0)
        );
    }
}

class Stats {

    private static final int MINIMUM_BLESSING_COUNT = 0;
    private static final int STATS_SIZE = 4;

    private final int str;
    private final int dex;
    private final int wiz;
    private final int luck;
    private final int targetAverageStat;

    public Stats(int str, int dex, int wiz, int luck, int targetAverageStat) {
        this.str = str;
        this.dex = dex;
        this.wiz = wiz;
        this.luck = luck;
        this.targetAverageStat = targetAverageStat;
    }

    public int getRequiredBlessingCount() {
        return Math.max(requiredBlessingCount(), MINIMUM_BLESSING_COUNT);
    }

    private int requiredBlessingCount() {
        return totalTargetStatCount() - sumStatsCount();
    }

    private int totalTargetStatCount() {
        return targetAverageStat * STATS_SIZE;
    }

    private int sumStatsCount() {
        return str + dex + wiz + luck;
    }
}
~~~
