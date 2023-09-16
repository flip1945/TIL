# 브실이의 띠부띠부씰 컬렉션 🍪 (Bronze 2)

### 문제 설명

<img src="https://upload.acmicpc.net/8dd922e4-0fc1-420e-b802-42dd6ed3d1e3/-/preview/" width="400px">

이 세상에서 두 번째로 맛있는 과자 브실칩이 있다.

하나의 브실칩에는 A부터 Z 중 하나의 알파벳 띠부띠부씰이 들어있다.

모은 띠부띠부씰들로 BRONZESILVER 글자를 만들어 우편으로 보내면 이 세상에서 제일 맛있는 골드칩 한 봉지를 배송해 준다고 한다.

지금까지 모은 띠부띠부씰들로 골드칩을 최대 몇 봉지 배송받을 수 있을지 계산해 보자.

출처 : https://www.acmicpc.net/problem/29713

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();

        Stickers stickers = new Stickers(scanner.nextLine());
        System.out.println(stickers.getCountOfGoldChip());
    }
}

class Stickers {

    public static final String GOLD_CHIP_STICKER = "BRONZESILVER";
    private final String stickers;

    public Stickers(String stickers) {
        this.stickers = stickers;
    }

    public int getCountOfGoldChip() {
        return getMinCountOfGoldChip(getCounter());
    }

    private Map<Character, Long> getCounter() {
        return stickers.chars()
                .mapToObj(c -> (char)c)
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    }

    private int getMinCountOfGoldChip(Map<Character, Long> counter) {
        int result = Integer.MAX_VALUE;
        for (char sticker : GOLD_CHIP_STICKER.toCharArray()) {
            result = Math.min(result, getStickerCount(counter, sticker));
        }
        return result;
    }

    private int getStickerCount(Map<Character, Long> counter, char sticker) {
        if (isTwiceAppearsSticker(sticker)) {
            return Math.toIntExact(counter.getOrDefault(sticker, 0L)) / 2;
        }
        return Math.toIntExact(counter.getOrDefault(sticker, 0L));
    }

    private boolean isTwiceAppearsSticker(char c) {
        return c == 'R' || c == 'E';
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

import java.util.Map;
import java.util.function.Function;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSticker")
    @DisplayName("스티커를 모아 몇 개의 골드 칩을 받을 수있는 지 계산해야 한다.")
    void shouldCountOfGoldChip(String stickers, int expected) {
        Stickers sut = new Stickers(stickers);

        int actual = sut.getCountOfGoldChip();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSticker() {
        return Stream.of(
                Arguments.of("BRONZESILVER", 1),
                Arguments.of("BRONZESILVE", 0),
                Arguments.of("BBBBBBBBBBBB", 0),
                Arguments.of("BRONZESILVERBRONZESILVER", 2),
                Arguments.of("BRONZESILVERBRONZESILVE", 1),
                Arguments.of("BRONZESILVERBRONZESILVR", 1),
                Arguments.of("BRONZESILVERRRRRR", 1)
        );
    }
}

class Stickers {

    public static final String GOLD_CHIP_STICKER = "BRONZESILVER";
    private final String stickers;

    public Stickers(String stickers) {
        this.stickers = stickers;
    }

    public int getCountOfGoldChip() {
        return getMinCountOfGoldChip(getCounter());
    }

    private Map<Character, Long> getCounter() {
        return stickers.chars()
                .mapToObj(c -> (char)c)
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    }

    private int getMinCountOfGoldChip(Map<Character, Long> counter) {
        int result = Integer.MAX_VALUE;
        for (char sticker : GOLD_CHIP_STICKER.toCharArray()) {
            result = Math.min(result, getStickerCount(counter, sticker));
        }
        return result;
    }

    private int getStickerCount(Map<Character, Long> counter, char sticker) {
        if (isTwiceAppearsSticker(sticker)) {
            return Math.toIntExact(counter.getOrDefault(sticker, 0L)) / 2;
        }
        return Math.toIntExact(counter.getOrDefault(sticker, 0L));
    }

    private boolean isTwiceAppearsSticker(char c) {
        return c == 'R' || c == 'E';
    }
}
~~~
