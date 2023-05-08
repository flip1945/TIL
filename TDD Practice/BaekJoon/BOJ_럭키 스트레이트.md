# 럭키 스트레이트 (Bronze 2)

### 문제 설명

어떤 게임의 아웃복서 캐릭터에게는 럭키 스트레이트라는 기술이 존재한다. 이 기술은 매우 강력한 대신에 항상 사용할 수는 없으며, 현재 게임 내에서 점수가 특정 조건을 만족할 때만 사용할 수 있다.

특정 조건이란 현재 캐릭터의 점수를 N이라고 할 때 점수 N을 자릿수를 기준으로 반으로 나누어 왼쪽 부분의 각 자릿수의 합과 오른쪽 부분의 각 자릿수의 합을 더한 값이 동일한 상황을 의미한다. 예를 들어 현재 점수가 123,402라면 왼쪽 부분의 각 자릿수의 합은 1+2+3, 오른쪽 부분의 각 자릿수의 합은 4+0+2이므로 두 합이 6으로 동일하여 럭키 스트레이트를 사용할 수 있다.

현재 점수 N이 주어졌을 때, 럭키 스트레이트를 사용할 수 있는 상태인지 아닌지를 알려주는 프로그램을 작성하시오. 럭키 스트레이트를 사용할 수 있다면 "LUCKY"를, 사용할 수 없다면 "READY"라는 단어를 출력한다. 또한 점수 N의 자릿수는 항상 짝수 형태로만 주어진다. 예를 들어 자릿수가 5인 12,345와 같은 수는 입력으로 들어오지 않는다.

출처 : https://www.acmicpc.net/problem/18406

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        LuckyStraightChecker checker = new LuckyStraightChecker();
        System.out.println(checker.canUseLuckyStraight(scanner.nextLine()));
    }
}

class LuckyStraightChecker {
    public String canUseLuckyStraight(String score) {
        int leftSum = getSumOfScoreDigit(getLeftDigit(score));
        int rightSum = getSumOfScoreDigit(getRightDigit(score));
        return (leftSum == rightSum) ? "LUCKY" : "READY";
    }

    private int getSumOfScoreDigit(String score) {
        return score.chars().map(Character::getNumericValue).sum();
    }

    private String getLeftDigit(String score) {
        return score.substring(0, score.length()/2);
    }

    private String getRightDigit(String score) {
        return score.substring(score.length()/2);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @CsvSource(value = {"11:LUCKY", "1221:LUCKY", "1234:READY", "123402:LUCKY", "7755:READY", "0000:LUCKY"}, delimiter = ':')
    @DisplayName("모든 D에 대해 D-유일하면 놀라운 문자열 아니라면 놀랍지 않은 문자열이라고 반환한다.")
    void lucky_straight_can_use_when_sum_of_left_and_right_digit_is_same(String score, String expected) {
        LuckyStraightChecker sut = new LuckyStraightChecker();

        String actual = sut.canUseLuckyStraight(score);

        assertEquals(expected, actual);
    }
}

class LuckyStraightChecker {
    public String canUseLuckyStraight(String score) {
        int leftSum = getSumOfScoreDigit(getLeftDigit(score));
        int rightSum = getSumOfScoreDigit(getRightDigit(score));
        return (leftSum == rightSum) ? "LUCKY" : "READY";
    }

    private int getSumOfScoreDigit(String score) {
        return score.chars().map(Character::getNumericValue).sum();
    }

    private String getLeftDigit(String score) {
        return score.substring(0, score.length()/2);
    }

    private String getRightDigit(String score) {
        return score.substring(score.length()/2);
    }
}
~~~
