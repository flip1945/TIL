# 너의 이름은 몇 점이니? (Bronze 2)

### 문제 설명

소윤이는 성필이에게 단단히 화가 났다. 성필이가 자꾸 소윤이의 이름을 놀리는 것이다!

극대노한 소윤이는 이름에 대해 많은 검색을 하던 도중 "이름점수"라는 것을 발견하게 된다. 이름 점수란, 알파벳 하나하나에 점수가 있고 이름에 들어가는 모든 알파벳 점수를 합한 것이라고 한다. 예를 들어 이름이 **SUNG PIL** 이라면,

* A = 1점
* B = 2점
* C = 3점
* ...
* Z = 26점

인 점수판에 따라 **S(19)+U(21) + N(14) + G(7) + P(16) + I(9) + L(12) = 98점**이다. (즉, 점수는 알파벳 순서이다) 

소윤이는 **SO YOON**이므로 **S(19) + O(15) + Y(25) + O(15) + O(15) + N(14) = 103점**으로 성필이보다 "이름점수"가 높았다! 그 사실을 알아챈 소윤이는 성필이에게 자신이 "이름점수"가 더 높다는 것을 전했고 성필이는 아직 충격에서 헤어나오지 못했다고 한다.

이제 소윤이는 사람의 이름을 볼 때 마다 "이름점수"를 계산해본다. 하지만 너무나 많은 사람을 만나기 때문에 계산하기가 귀찮다! 귀찮아진 소윤이를 위해 "이름점수"를 계산하는 프로그램을 만들어 주자.

출처 : https://www.acmicpc.net/problem/15813

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String n = scanner.nextLine();
        String name = scanner.nextLine();

        System.out.println(new NameScorer(name).calculate());
    }
}

class NameScorer {
    private static final int UPPERCASE_OFFSET = 64;

    private final String name;

    public NameScorer(String name) {
        this.name = name;
    }

    public int calculate() {
        return name.chars()
                .map(this::getScoreOfLetter)
                .sum();
    }

    private int getScoreOfLetter(int letter) {
        return letter - UPPERCASE_OFFSET;
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
    @MethodSource("provideNames")
    @DisplayName("이름점수를 반환한다.")
    void calculateNameScoreTest(String name, int expected) {
        NameScorer sut = new NameScorer(name);

        int actual = sut.calculate();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNames() {
        return Stream.of(
                Arguments.of("A", 1)
                , Arguments.of("B", 2)
                , Arguments.of("C", 3)
                , Arguments.of("Z", 26)
                , Arguments.of("ABC", 6)
                , Arguments.of("SUNGPIL", 98)
                , Arguments.of("SOYOON", 103)
        );
    }
}

class NameScorer {
    private static final int UPPERCASE_OFFSET = 64;

    private final String name;

    public NameScorer(String name) {
        this.name = name;
    }

    public int calculate() {
        return name.chars()
                .map(this::getScoreOfLetter)
                .sum();
    }

    private int getScoreOfLetter(int letter) {
        return letter - UPPERCASE_OFFSET;
    }
}
~~~
