# Goodbye, Code Jam (Bronze 4)

### 문제 설명

매년 4~5월은 Google Code Jam이 열리는 기간이다. 슬프게도 Google은 2023년부터 Code Jam, Kick Start, Hash Code 등 다양한 형태로 운영해 왔던 Google Coding Competition을 종료하기로 했다. 더 이상 티셔츠를 받을 수 없다는 사실에 절망한 브실이는 Google Code Jam을 기억하는 문제를 만들기로 했다.

Google Code Jam은 Qualification Round, Round 1, Round 2, Round 3, World Finals의 순서로 진행된다. 30점 이상을 획득하면 다음 라운드로 진출하는 Qualification Round와 더 이상 다음 라운드가 없는 World Finals을 제외한 라운드는 다음 라운드에 진출하기 위해선 정해진 순위 안에 들어야 한다. 각각 Round 1은 상위 $4\,500$등, Round 2는 상위 $1\,000$등, Round 3은 상위 $25$등 안에 들어야 다음 라운드에 진출할 수 있다.

입력으로 Google Code Jam 참가자가 가장 마지막으로 참가한 라운드의 등수 $N$이 주어진다. 해당 참가자가 가장 마지막으로 참가한 라운드를 출력하라. 단 문제에서 주어지는 참가자는 Qualification Round는 모두 통과했다고 가정하므로 출력해야 할 라운드는 Round 1, Round 2, Round 3, World Finals 중 하나이다. 모든 참가자는 Google Code Jam에 참가하기만을 손꼽아 기다려 왔기 때문에 참가자가 등수 미달로 탈락하는 경우가 아닌 이상 중도 포기를 하는 경우는 없다.

출처 : https://www.acmicpc.net/problem/29738

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = scanner.nextInt();
        GoogleCodeJam googleCodeJam = new GoogleCodeJam();
        for (int i = 1; i <= t; i++) {
            int rank = scanner.nextInt();
            String round = googleCodeJam.getFinalRound(rank);

            System.out.println("Case #" + i + ": " + round);
        }
    }
}

class GoogleCodeJam {

    public String getFinalRound(int rank) {
        return CodeJamRound.from(rank).getRoundName();
    }
}

enum CodeJamRound {
    ROUND1("Round 1", rank -> false),
    ROUND2("Round 2", rank -> rank <= 4500 && rank > 1000),
    ROUND3("Round 3", rank -> rank <= 1000 && rank > 25),
    WORLD_FINALS("World Finals", rank -> rank <= 25 && rank > 0);

    private final String roundName;
    private final Predicate<Integer> cutOffRank;

    CodeJamRound(String roundName, Predicate<Integer> cutOffRank) {
        this.roundName = roundName;
        this.cutOffRank = cutOffRank;
    }

    public static CodeJamRound from(int rank) {
        return Arrays.stream(values())
                .filter(value -> value.cutOffRank.test(rank))
                .findFirst()
                .orElse(ROUND1);
    }

    public String getRoundName() {
        return roundName;
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

import java.util.Arrays;
import java.util.function.Predicate;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideRank")
    @DisplayName("참가자가 가장 마지막으로 참가한 라운드를 반환한다.")
    void shouldReturnFinalRound(int rank, String expected) {
        GoogleCodeJam sut = new GoogleCodeJam();

        String actual = sut.getFinalRound(rank);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideRank() {
        return Stream.of(
                Arguments.of(30000, "Round 1"),
                Arguments.of(10000, "Round 1"),
                Arguments.of(4500, "Round 2"),
                Arguments.of(1234, "Round 2"),
                Arguments.of(1000, "Round 3"),
                Arguments.of(567, "Round 3"),
                Arguments.of(25, "World Finals"),
                Arguments.of(8, "World Finals"),
                Arguments.of(1, "World Finals")
        );
    }
}

class GoogleCodeJam {

    public String getFinalRound(int rank) {
        return CodeJamRound.from(rank).getRoundName();
    }
}

enum CodeJamRound {
    ROUND1("Round 1", rank -> false),
    ROUND2("Round 2", rank -> rank <= 4500 && rank > 1000),
    ROUND3("Round 3", rank -> rank <= 1000 && rank > 25),
    WORLD_FINALS("World Finals", rank -> rank <= 25 && rank > 0);

    private final String roundName;
    private final Predicate<Integer> cutOffRank;

    CodeJamRound(String roundName, Predicate<Integer> cutOffRank) {
        this.roundName = roundName;
        this.cutOffRank = cutOffRank;
    }

    public static CodeJamRound from(int rank) {
        return Arrays.stream(values())
                .filter(value -> value.cutOffRank.test(rank))
                .findFirst()
                .orElse(ROUND1);
    }

    public String getRoundName() {
        return roundName;
    }
}
~~~
