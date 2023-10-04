# :chino_shock: (Bronze 4)

### 문제 설명

sait2000은 아니메컵 서버를 열고 나서 가장 먼저 이모지 :chino_shock:부터 추가했다. :chino_shock:를 비롯한 여러 이모지를 사용해보던 sait2000은 :chino_shock:와 같이 이름에 언더바(_)가 들어간 이모지가 :chinoaww:와 같이 이름에 언더바가 들어가지 않은 이모지보다 더 타이핑하기 어렵다는 사실을 알게 되었다.

이모지는 하나의 콜론(:)으로 시작해서 하나의 콜론으로 끝나며, 콜론 사이의 문자들은 모두 영어 소문자 혹은 언더바(_) 중 하나로 주어진다. sait2000은 이모지의 입력 난이도를 이모지의전체길이콜론의개수언더바의개수$(\text{이모지의 전체 길이}) +(\text{콜론의 개수}) +(\text{언더바의 개수})\times 5$로 정의했다. 이 정의에 따르면 :chino_shock:의 전체 길이는 $13$이고 콜론이 $2$개, 언더바가 $1$개이므로 입력 난이도가 $13+2+1\times 5=20$이 된다.

이모지가 주어졌을 때 이모지의 입력 난이도를 계산해 출력해보자.

출처 : https://www.acmicpc.net/problem/27310

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Emoji emoji = new Emoji(scanner.nextLine());
        System.out.println(emoji.getDifficulty());
    }
}

class Emoji {

    private static final int COLON_SIZE = 2;
    private static final int UNDER_BAR_SCORE = 5;
    private static final char UNDER_BAR = '_';

    private final String emoji;

    public Emoji(String emoji) {
        this.emoji = emoji;
    }

    public int getDifficulty() {
        return emoji.length() + COLON_SIZE + (getCountOfUnderBar() * UNDER_BAR_SCORE);
    }

    private int getCountOfUnderBar() {
        return (int) emoji.chars()
                .filter(ch -> ch == UNDER_BAR)
                .count();
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
    @MethodSource("provideEmoji")
    @DisplayName("이모지는 난이도를 계산한다.")
    void emojiCalculateDifficulty(String emoji, int expected) {
        Emoji sut = new Emoji(emoji);

        int actual = sut.getDifficulty();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideEmoji() {
        return Stream.of(
                Arguments.of(":a:", 5),
                Arguments.of(":ab:", 6),
                Arguments.of(":a_b:", 12),
                Arguments.of(":chino_shock:", 20),
                Arguments.of(":chino_very_shock:", 30)
        );
    }
}

class Emoji {

    private static final int COLON_SIZE = 2;
    private static final int UNDER_BAR_SCORE = 5;
    private static final char UNDER_BAR = '_';

    private final String emoji;

    public Emoji(String emoji) {
        this.emoji = emoji;
    }

    public int getDifficulty() {
        return emoji.length() + COLON_SIZE + (getCountOfUnderBar() * UNDER_BAR_SCORE);
    }

    private int getCountOfUnderBar() {
        return (int) emoji.chars()
                .filter(ch -> ch == UNDER_BAR)
                .count();
    }
}
~~~
