# 상금 헌터 (Bronze 3)

### 문제 설명

2017년에 이어, 2018년에도 카카오 코드 페스티벌이 개최된다!

<img src="https://upload.acmicpc.net/0113dbfe-8ca8-42b8-9a2c-94e136006b75/-/preview/">

카카오 코드 페스티벌에서 빠질 수 없는 것은 바로 상금이다. 2017년에 개최된 제1회 코드 페스티벌에서는, 본선 진출자 100명 중 21명에게 아래와 같은 기준으로 상금을 부여하였다.

|순위|	상금|	인원|
|-|-|-|
|1등|	500만원|	1명|
|2등|	300만원|	2명|
|3등|	200만원|	3명|
|4등|	50만원|	4명|
|5등|	30만원|	5명|
|6등|	10만원|	6명|

2018년에 개최될 제2회 코드 페스티벌에서는 상금의 규모가 확대되어, 본선 진출자 64명 중 31명에게 아래와 같은 기준으로 상금을 부여할 예정이다.

|순위|	상금|	인원|
|-|-|-|
|1등|	512만원	|1명|
|2등|	256만원	|2명|
|3등|	128만원	|4명|
|4등|	64만원	|8명|
|5등|	32만원	|16명|

<img src="https://upload.acmicpc.net/2ff64533-7387-4294-8dce-03ba3d35b7d4/-/preview/">

제이지는 자신이 코드 페스티벌에 출전하여 받을 수 있을 상금이 얼마인지 궁금해졌다. 그는 자신이 두 번의 코드 페스티벌 본선 대회에서 얻을 수 있을 총 상금이 얼마인지 알아보기 위해, 상상력을 발휘하여 아래와 같은 가정을 하였다.

* 제1회 코드 페스티벌 본선에 진출하여 a등(1 ≤ a ≤ 100)등을 하였다. 단, 진출하지 못했다면 a = 0으로 둔다.
* 제2회 코드 페스티벌 본선에 진출하여 b등(1 ≤ b ≤ 64)등을 할 것이다. 단, 진출하지 못했다면 b = 0으로 둔다.

제이지는 이러한 가정에 따라, 자신이 받을 수 있는 총 상금이 얼마인지를 알고 싶어한다.

출처 : https://www.acmicpc.net/problem/15953

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            CodeFestival codeFestival = new CodeFestival();
            System.out.println(codeFestival.getPrize(scanner.nextInt(), scanner.nextInt()));
        }
    }
}

class CodeFestival {

    private final FirstCodeFestival firstCodeFestival = new FirstCodeFestival();
    private final SecondCodeFestival secondCodeFestival = new SecondCodeFestival();

    public int getPrize(int firstCodeFestivalRank, int secondCodeFestivalRank) {
        return firstCodeFestival.getPrize(firstCodeFestivalRank) + secondCodeFestival.getPrize(secondCodeFestivalRank);
    }
}

class FirstCodeFestival {

    public int getPrize(int rank) {
        return Prize.from(rank).getPrize();
    }

    enum Prize {
        FIRST_PRIZE(5_000_000, rank -> rank == 1),
        SECOND_PRIZE(3_000_000, rank -> rank > 1 && rank <= 3),
        THIRD_PRIZE(2_000_000, rank -> rank > 3 && rank <= 6),
        FOURTH_PRIZE(500_000, rank -> rank > 6 && rank <= 10),
        FIFTH_PRIZE(300_000, rank -> rank > 10 && rank <= 15),
        SIXTH_PRIZE(100_000, rank -> rank > 15 && rank <= 21),
        NOT_PRIZE(0, rank -> false);

        private final int prize;
        private final Predicate<Integer> range;

        Prize(int prize, Predicate<Integer> range) {
            this.prize = prize;
            this.range = range;
        }

        public static Prize from(int rank) {
            for (Prize value : values()) {
                if (value.range.test(rank)) {
                    return value;
                }
            }
            return NOT_PRIZE;
        }

        public int getPrize() {
            return prize;
        }
    }
}

class SecondCodeFestival {

    public int getPrize(int rank) {
        return Prize.from(rank).getPrize();
    }

    enum Prize {
        FIRST_PRIZE(5_120_000, rank -> rank == 1),
        SECOND_PRIZE(2_560_000, rank -> rank > 1 && rank <= 3),
        THIRD_PRIZE(1_280_000, rank -> rank > 3 && rank <= 7),
        FOURTH_PRIZE(640_000, rank -> rank > 7 && rank <= 15),
        FIFTH_PRIZE(320_000, rank -> rank > 15 && rank <= 31),
        NOT_PRIZE(0, rank -> false);

        private final int prize;
        private final Predicate<Integer> range;

        Prize(int prize, Predicate<Integer> range) {
            this.prize = prize;
            this.range = range;
        }

        public static Prize from(int rank) {
            for (Prize value : values()) {
                if (value.range.test(rank)) {
                    return value;
                }
            }
            return NOT_PRIZE;
        }

        public int getPrize() {
            return prize;
        }
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

import java.util.function.Predicate;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideCodeFestivalRanks")
    @DisplayName("코드 페스티벌 순위들이 주어지면 상금을 반환한다.")
    void shouldReturnCodeFestivalPrize(int firstCodeFestivalRank, int secondCodeFestivalRank, int expected) {
        CodeFestival sut = new CodeFestival();

        int actual = sut.getPrize(firstCodeFestivalRank, secondCodeFestivalRank);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCodeFestivalRanks() {
        return Stream.of(
                Arguments.of(8, 4, 1780000),
                Arguments.of(13, 19, 620000),
                Arguments.of(8, 10, 1140000),
                Arguments.of(18, 18, 420000),
                Arguments.of(8, 25, 820000),
                Arguments.of(13, 16, 620000)
        );
    }

    @ParameterizedTest
    @MethodSource("provideFirstCodeFestivalRanks")
    @DisplayName("첫번째 코드 페스티벌 순위가 주어지면 상금을 반환한다.")
    void shouldReturnFirstCodeFestivalPrize(int rank, int expected) {
        FirstCodeFestival sut = new FirstCodeFestival();

        int actual = sut.getPrize(rank);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideFirstCodeFestivalRanks() {
        return Stream.of(
                Arguments.of(1, 5_000_000),
                Arguments.of(2, 3_000_000),
                Arguments.of(3, 3_000_000),
                Arguments.of(4, 2_000_000),
                Arguments.of(6, 2_000_000),
                Arguments.of(7, 500_000),
                Arguments.of(10, 500_000),
                Arguments.of(11, 300_000),
                Arguments.of(15, 300_000),
                Arguments.of(16, 100_000),
                Arguments.of(21, 100_000),
                Arguments.of(22, 0),
                Arguments.of(0, 0),
                Arguments.of(100, 0)
        );
    }

    @ParameterizedTest
    @MethodSource("provideSecondCodeFestivalRank")
    @DisplayName("두번째 코드 페스티벌 순위가 주어지면 상금을 반환한다.")
    void shouldReturnSecondCodeFestivalPrize(int rank, int expected) {
        SecondCodeFestival sut = new SecondCodeFestival();

        int actual = sut.getPrize(rank);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSecondCodeFestivalRank() {
        return Stream.of(
                Arguments.of(1, 5_120_000),
                Arguments.of(2, 2_560_000),
                Arguments.of(3, 2_560_000),
                Arguments.of(4, 1_280_000),
                Arguments.of(7, 1_280_000),
                Arguments.of(8, 640_000),
                Arguments.of(15, 640_000),
                Arguments.of(16, 320_000),
                Arguments.of(31, 320_000),
                Arguments.of(32, 0),
                Arguments.of(0, 0),
                Arguments.of(100, 0)
        );
    }
}

class CodeFestival {

    private final FirstCodeFestival firstCodeFestival = new FirstCodeFestival();
    private final SecondCodeFestival secondCodeFestival = new SecondCodeFestival();

    public int getPrize(int firstCodeFestivalRank, int secondCodeFestivalRank) {
        return firstCodeFestival.getPrize(firstCodeFestivalRank) + secondCodeFestival.getPrize(secondCodeFestivalRank);
    }
}

class FirstCodeFestival {

    public int getPrize(int rank) {
        return Prize.from(rank).getPrize();
    }

    enum Prize {
        FIRST_PRIZE(5_000_000, rank -> rank == 1),
        SECOND_PRIZE(3_000_000, rank -> rank > 1 && rank <= 3),
        THIRD_PRIZE(2_000_000, rank -> rank > 3 && rank <= 6),
        FOURTH_PRIZE(500_000, rank -> rank > 6 && rank <= 10),
        FIFTH_PRIZE(300_000, rank -> rank > 10 && rank <= 15),
        SIXTH_PRIZE(100_000, rank -> rank > 15 && rank <= 21),
        NOT_PRIZE(0, rank -> false);

        private final int prize;
        private final Predicate<Integer> range;

        Prize(int prize, Predicate<Integer> range) {
            this.prize = prize;
            this.range = range;
        }

        public static Prize from(int rank) {
            for (Prize value : values()) {
                if (value.range.test(rank)) {
                    return value;
                }
            }
            return NOT_PRIZE;
        }

        public int getPrize() {
            return prize;
        }
    }
}

class SecondCodeFestival {

    public int getPrize(int rank) {
        return Prize.from(rank).getPrize();
    }

    enum Prize {
        FIRST_PRIZE(5_120_000, rank -> rank == 1),
        SECOND_PRIZE(2_560_000, rank -> rank > 1 && rank <= 3),
        THIRD_PRIZE(1_280_000, rank -> rank > 3 && rank <= 7),
        FOURTH_PRIZE(640_000, rank -> rank > 7 && rank <= 15),
        FIFTH_PRIZE(320_000, rank -> rank > 15 && rank <= 31),
        NOT_PRIZE(0, rank -> false);

        private final int prize;
        private final Predicate<Integer> range;

        Prize(int prize, Predicate<Integer> range) {
            this.prize = prize;
            this.range = range;
        }

        public static Prize from(int rank) {
            for (Prize value : values()) {
                if (value.range.test(rank)) {
                    return value;
                }
            }
            return NOT_PRIZE;
        }

        public int getPrize() {
            return prize;
        }
    }
}
~~~
