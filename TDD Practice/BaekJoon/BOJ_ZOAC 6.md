# ZOAC 6 (Bronze 4)

### 문제 설명

2023년 9월, 여섯 번째로 개최된 ZOAC의 오프닝을 또 맡은 성우는 영과일의 마스코트인 영일이를 이용해 대회를 홍보하기로 했다.

성우는 홍보 글이 주어질 때 각 문장에 01 또는 OI가 포함되어 있다면 문장 끝에 한 개의 영일이 이모티콘을 넣기로 했다. 이때, 홍보 글에 영일이 이모티콘을 총 몇 번 넣어야 하는지 구하여라.

<img src="https://upload.acmicpc.net/a1376dea-4d12-4c21-bab6-8eeb5eeb4b1f/-/crop/3876x2707/0,181/-/preview/" width="150px">

출처 : https://www.acmicpc.net/problem/30045

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        PromotionPosters promotionPosters = PromotionPosters.from(inputPromotionPosters(scanner, n));
        System.out.println(promotionPosters.countOfYoungIlEmotion());
    }

    private static List<String> inputPromotionPosters(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class PromotionPosters {

    private final List<PromotionPoster> promotionPosters;

    public PromotionPosters(List<PromotionPoster> promotionPosters) {
        this.promotionPosters = promotionPosters;
    }

    public static PromotionPosters from(List<String> promotionPosters) {
        return new PromotionPosters(initPromotionPosters(promotionPosters));
    }

    private static List<PromotionPoster> initPromotionPosters(List<String> promotionPosters) {
        return promotionPosters.stream()
                .map(PromotionPoster::new)
                .collect(Collectors.toList());
    }

    public int countOfYoungIlEmotion() {
        return (int) promotionPosters.stream()
                .filter(PromotionPoster::containsYoungIl)
                .count();
    }
}

class PromotionPoster {

    private static final String YOUNG_IL_NUMBER = "01";
    private static final String YOUNG_IL_STRING = "OI";

    private final String content;

    public PromotionPoster(String content) {
        this.content = content;
    }

    public boolean containsYoungIl() {
        return content.contains(YOUNG_IL_NUMBER) || content.contains(YOUNG_IL_STRING);
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

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePromotionPosters")
    @DisplayName("홍보글들은 영일 이모티콘이 포함된 홍보글의 개수를 카운트한다.")
    void promotionPostersGetCountOfYoungIlEmotion(List<String> promotionPosters, int expected) {
        PromotionPosters sut = PromotionPosters.from(promotionPosters);

        int actual = sut.countOfYoungIlEmotion();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> providePromotionPosters() {
        return Stream.of(
                Arguments.of(List.of("a"), 0),
                Arguments.of(List.of("01"), 1),
                Arguments.of(List.of("OI"), 1),
                Arguments.of(List.of("2015"), 1),
                Arguments.of(List.of("IOI"), 1),
                Arguments.of(List.of("IOI", "2015"), 2),
                Arguments.of(List.of("JOJ", "2023"), 0)
        );
    }
}

class PromotionPosters {

    private final List<PromotionPoster> promotionPosters;

    public PromotionPosters(List<PromotionPoster> promotionPosters) {
        this.promotionPosters = promotionPosters;
    }

    public static PromotionPosters from(List<String> promotionPosters) {
        return new PromotionPosters(initPromotionPosters(promotionPosters));
    }

    private static List<PromotionPoster> initPromotionPosters(List<String> promotionPosters) {
        return promotionPosters.stream()
                .map(PromotionPoster::new)
                .collect(Collectors.toList());
    }

    public int countOfYoungIlEmotion() {
        return (int) promotionPosters.stream()
                .filter(PromotionPoster::containsYoungIl)
                .count();
    }
}

class PromotionPoster {

    private static final String YOUNG_IL_NUMBER = "01";
    private static final String YOUNG_IL_STRING = "OI";

    private final String content;

    public PromotionPoster(String content) {
        this.content = content;
    }

    public boolean containsYoungIl() {
        return content.contains(YOUNG_IL_NUMBER) || content.contains(YOUNG_IL_STRING);
    }
}
~~~
