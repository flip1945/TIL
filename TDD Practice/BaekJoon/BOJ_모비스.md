# 모비스 (Bronze 4)

### 문제 설명

현대 모비스의 MOBIS는 어떤 뜻을 가지고 있을까?

MOBIS는 기존에는 Mobile + System의 합성어에서 시작되었지만, 현재는 "Mobility Beyond Integrated Solution" 라는 의미로 재정의 되었다.

이는 사용자의 경험을 혁신하고, 고객의 요구에 최적화된 통합 솔루션, 그 이상의 가치를 전달하는 모빌리티 플랫폼 프로바이더로 도약하겠다는 뜻을 가지고 있다.

이 뜻에 매료된 진익이는 스티커 용지에 인쇄되어 있는 문자들 중 'M', 'O', 'B', 'I', 'S' 만을 오리고 적절히 배치하여 노트북에 MOBIS를 붙여놓고자 한다.

스티커 용지에 인쇄되어 있는 문자열이 주어진다. 이 문자들을 이용해 MOBIS를 만들 수 있을까?

출처 : https://www.acmicpc.net/problem/28074

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Mobis mobis = new Mobis(scanner.nextLine());
        System.out.println(mobis.check());
    }
}

class Mobis {

    private static final String CAN = "YES";
    private static final String CAN_NOT = "NO";

    private final String word;

    public Mobis(String word) {
        this.word = word;
    }

    public String check() {
        return canCreate(word) ? CAN : CAN_NOT;
    }

    private boolean canCreate(String word) {
        return word.contains("M") && word.contains("O") && word.contains("B") && word.contains("I")
                && word.contains("S");
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
    @MethodSource("provideWord")
    @DisplayName("주어진 단어로 MOBIS를 만들 수 있으면 YES, 아니라면 NO를 반환한다.")
    void mobisChecksThatItCanCreateOrNotAMobis(String word, String expected) {
        Mobis sut = new Mobis(word);

        String actual = sut.check();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideWord() {
        return Stream.of(
                Arguments.of("MOBIS", "YES"),
                Arguments.of("A", "NO"),
                Arguments.of("ABCDE", "NO"),
                Arguments.of("MMOBIS", "YES")
        );
    }
}

class Mobis {

    private static final String CAN = "YES";
    private static final String CAN_NOT = "NO";

    private final String word;

    public Mobis(String word) {
        this.word = word;
    }

    public String check() {
        return canCreate(word) ? CAN : CAN_NOT;
    }

    private boolean canCreate(String word) {
        return word.contains("M") && word.contains("O") && word.contains("B") && word.contains("I")
                && word.contains("S");
    }
}
~~~
