# 몇개고? (Bronze 5)

### 문제 설명

고려대학교 로봇융합관에서 MatKor Cup을 준비하던 주영이는 같이 초밥을 먹자는 동우의 말에 호랭이 초밥 집에 갔다. 모듬 초밥을 먹으면서 동우와 주영이는 다음과 같은 대화를 하였다.

* 동우: "몇개고?"
* 주영: "응?"
* 동우: "밥알말이다. 몇개고?"
* 주영: "그건 또 뭔데?"
* 동우: "삼백 이십개다. 훈련된 초밥 장인이 이 한번 스시를 쥘 때 보통은 이 밥알이 삼백 이십개라. 점심 식사에는 삼백 이십개가 적당하다 캐도, 오늘 같은 날이나 술하고 같이 낼 때는 이백 팔십개만 해라, 어이? 배 안부르구로"
* 주영: "어디서 또 이상한거 배워왔냐"
* 동우: "너 혹시 재벌집 막내아들 뭔지 모르나?"
* 주영: "모른다"

대한민국을 뒤흔든 드라마를 모른다는 주영이의 말에 동우는 적잖은 충격을 받았다. 사태의 심각성을 느낀 동우는 주영이가 '재벌집 막내아들'을 보게 하기 위해 MatKor 사람들을 모았다. 주영이에게 그냥 영상을 보여주는 것보다 알고리즘을 이용해서 알려주어야 흥미를 가지게 할 수 있다고 생각한 MatKor 사람들은 주영이가 드라마에 흥미를 가지게 하기 초밥 밥알 갯수로 문제를 만들기로 했다. 자 MatKor에서 문제를 만들었고, 동우가 주영이에게 문제를 말했다.

* 동우: "오늘 나는 너가 만든 초밥을 먹을 거야. 너는 '재벌집 막내아들'의 진양철 회장님의 말에 따라 **술하고 같이 초밥을 먹거나 점심 식사가 아닐 때는 초밥의 밥알을 $280$개로** 하며, **점심 식사이면서 술과 같이 먹지 않을때는 초밥의 밥알을 $320$개**로 하여 초밥을 만들어야 해, 근데, 내가 초밥을 언제 먹을지, 혹은 술과 같이 먹을지 아직 정하지 않았어. 내가 초밥을 먹는 시간과 술의 유무를 말하면, 그때 너는 너가 만들어야 하는 초밥의 밥알 갯수를 구해야해"

당신은 위의 문제를 읽고 주영이를 대신하여 동우의 문제를 해결하여라.

출처 : https://www.acmicpc.net/problem/27294

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Time time = new Time(scanner.nextInt());
        Liquor liquor = new Liquor(scanner.nextInt());

        Sushi sushi = new Sushi(time, liquor);
        System.out.println(sushi.getSize());
    }
}

class Sushi {

    private final Time time;
    private final Liquor liquor;

    public Sushi(Time time, Liquor liquor) {
        this.time = time;
        this.liquor = liquor;
    }

    public int getSize() {
        if (needLargeSizeSushi()) {
            return SushiSize.LARGE.getSize();
        }
        return SushiSize.SMALL.getSize();
    }

    private boolean needLargeSizeSushi() {
        return time.isLunchTime() && !liquor.withMeals();
    }
}

class Time {

    private static final int START_LUNCH_TIME = 12;
    private static final int END_LUNCH_TIME = 16;

    private final int time;

    public Time(int time) {
        this.time = time;
    }

    public boolean isLunchTime() {
        return time >= START_LUNCH_TIME && time <= END_LUNCH_TIME;
    }
}

class Liquor {

    private static final int WITH_MEALS = 1;

    private final int liquorWithMeals;

    public Liquor(int liquorWithMeals) {
        this.liquorWithMeals = liquorWithMeals;
    }

    public boolean withMeals() {
        return liquorWithMeals == WITH_MEALS;
    }
}

enum SushiSize {
    LARGE(320),
    SMALL(280);

    private final int size;

    SushiSize(int size) {
        this.size = size;
    }

    public int getSize() {
        return size;
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
    @MethodSource("provideSmallSizeTimeAndLiquor")
    @DisplayName("초밥은 술하고 같이 먹거나 점심 시간이 아닐 경우 작은 사이즈로 만든다.")
    void sushiMakeSmallSizeGivenWithLiquorOrNotLunchTime(int time, int liquor) {
        Sushi sut = new Sushi(new Time(time), new Liquor(liquor));

        int actual = sut.getSize();

        assertEquals(280, actual);
    }

    private static Stream<Arguments> provideSmallSizeTimeAndLiquor() {
        return Stream.of(
                Arguments.of(8, 0),
                Arguments.of(10, 0),
                Arguments.of(17, 0),
                Arguments.of(18, 0),
                Arguments.of(10, 1),
                Arguments.of(12, 1),
                Arguments.of(16, 1)
        );
    }

    @ParameterizedTest
    @MethodSource("provideLargeSizeTimeAndLiquor")
    @DisplayName("초밥은 술하고 같이 먹거나 점심 시간이 아닐 경우 작은 사이즈로 만든다.")
    void sushiMakeLargeSizeGivenWithNotLiquorAndLunchTime(int time, int liquor) {
        Sushi sut = new Sushi(new Time(time), new Liquor(liquor));

        int actual = sut.getSize();

        assertEquals(320, actual);
    }

    private static Stream<Arguments> provideLargeSizeTimeAndLiquor() {
        return Stream.of(
                Arguments.of(12, 0),
                Arguments.of(16, 0)
        );
    }
}

class Sushi {

    private final Time time;
    private final Liquor liquor;

    public Sushi(Time time, Liquor liquor) {
        this.time = time;
        this.liquor = liquor;
    }

    public int getSize() {
        if (needLargeSizeSushi()) {
            return SushiSize.LARGE.getSize();
        }
        return SushiSize.SMALL.getSize();
    }

    private boolean needLargeSizeSushi() {
        return time.isLunchTime() && !liquor.withMeals();
    }
}

class Time {

    private static final int START_LUNCH_TIME = 12;
    private static final int END_LUNCH_TIME = 16;

    private final int time;

    public Time(int time) {
        this.time = time;
    }

    public boolean isLunchTime() {
        return time >= START_LUNCH_TIME && time <= END_LUNCH_TIME;
    }
}

class Liquor {

    private static final int WITH_MEALS = 1;

    private final int liquorWithMeals;

    public Liquor(int liquorWithMeals) {
        this.liquorWithMeals = liquorWithMeals;
    }

    public boolean withMeals() {
        return liquorWithMeals == WITH_MEALS;
    }
}

enum SushiSize {
    LARGE(320),
    SMALL(280);

    private final int size;

    SushiSize(int size) {
        this.size = size;
    }

    public int getSize() {
        return size;
    }
}
~~~
