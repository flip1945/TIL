# Betting (Bronze 4)

### 문제 설명

The popular streaming platform switch.tv just unveiled their newest feature: switch betting.  Streamers can now get their viewers to bet on two different options using switch points (patented).

Each viewer bets some number of switch points for one of the two options. The total amount of switch points bet by everyone is called the prize pool. The streamer will choose one of the options as the winner and the prize pool is split (not necessarily equally) between all the viewers who bet on that option; the more you bet on the option, the more of the prize pool you receive. In particular, if you contributed $p\%$ of all the bets for one of the options and that option wins, then you receive $p\%$ of the total prize pool.

The switch.tv team has come to you to compute what the switch point payout is for each viewer if their selected option wins. To do this, they ask you to find the switch-payout-ratio for each of the two options. Since the payout to each viewer is proportional to the number of switch points they put into the bet, the switch team will be able to use this ratio to determine each viewer's winnings.

For example, suppose a streamer created a switch bet where three viewers participated. Two viewers bet $10$ and $30$ switch points on option one and the last viewer bets $10$ switch points on option two. We can see that option one has $80\%$ of the bets and option two has $20\%$ of the bets.

If option one wins, then the two viewers who bet on that receive $12.5$ and $37.5$ switch points, respectively, which means that the switch-payout-ratio is $1$ : $1.25$ for option one. If option two wins, then the single viewer who bets on that receives 50 switch points, which means that the switch-payout-ratio is $1$ : $5$.

Given the percentage of total bets for option one, help switch.tv by computing the switch-payout-ratio for the two options.

출처 : https://www.acmicpc.net/problem/24751

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Betting betting = new Betting(scanner.nextInt());
        System.out.println(betting.getOption1Dividend());
        System.out.println(betting.getOption2Dividend());
    }
}

class Betting {

    private static final int PERCENT = 100;

    private final int option1Rating;

    public Betting(int option1Rating) {
        this.option1Rating = option1Rating;
    }

    public double getOption1Dividend() {
        return (double) PERCENT / option1Rating;
    }

    public double getOption2Dividend() {
        return (double) PERCENT / getOption2Rating();
    }

    private int getOption2Rating() {
        return PERCENT - option1Rating;
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
    @MethodSource("provideOption1Rating")
    @DisplayName("1번 옵션의 비율이 주어지면 각 옵션의 배당을 반환한다.")
    void bettingReturnsEachOptionsDividendGivenOption1Rating(int option1Rating, double expected1, double expected2) {
        Betting sut = new Betting(option1Rating);

        assertEquals(expected1, sut.getOption1Dividend(), 0.000001);
        assertEquals(expected2, sut.getOption2Dividend(), 0.000001);
    }

    static Stream<Arguments> provideOption1Rating() {
        return Stream.of(
                Arguments.of(80, 1.25, 5.0),
                Arguments.of(50, 2.0, 2.0),
                Arguments.of(15, 6.6666666667, 1.1764705882)
        );
    }
}

class Betting {

    private static final int PERCENT = 100;

    private final int option1Rating;

    public Betting(int option1Rating) {
        this.option1Rating = option1Rating;
    }

    public double getOption1Dividend() {
        return (double) PERCENT / option1Rating;
    }

    public double getOption2Dividend() {
        return (double) PERCENT / getOption2Rating();
    }

    private int getOption2Rating() {
        return PERCENT - option1Rating;
    }
}
~~~
