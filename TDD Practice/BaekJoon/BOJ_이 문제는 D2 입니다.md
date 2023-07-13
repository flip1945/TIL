# 이 문제는 D2 입니다 (Bronze 2)

### 문제 설명

<img src='https://upload.acmicpc.net/767c568a-5e3b-4dcf-9dc4-93cc1a0b11fc/' width='350px'>

올해도 연세대학교 프로그래밍 경진대회가 열렸다!

병철이는 원래 대회에 출전하여 상금을 타갈 생각을 하고 있었으나, 갑자기 출제진으로 끌려가게 되어버렸다.

출제진으로 끌려온 병철이에게 연세대학교 프로그래밍 경진대회는 지원을 받지 못하면 출제진이 사비로 상금을 지급해야 한다는 충격적인 소식이 들려왔지만, 다행히 지원을 받았기에 그런 대참사는 막을 수 있었다.

지갑이 털릴 위기를 벗어난 병철이는 바로 백준을 켜 
$D2$ 난이도의 문제를 풀었고, 대회 참가자들에게도 
$D2$ 난이도의 문제를 내려고 한다!

그러나 실력이 딸려 
$D2$ 난이도의 문제를 낼 순 없었기에, 그냥 문제 이름을 
$D2$로 했다.

여러분들도 (이름만) 
$D2$급 문제를 풀어보자!

출처 : https://www.acmicpc.net/problem/11170

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        D2Word d2Word = new D2Word(new D2WordChecker());
        System.out.println(d2Word.isD2Word(scanner.nextLine()));
    }
}

class D2Word {

    private final D2WordChecker d2WordChecker;

    public D2Word(D2WordChecker d2WordChecker) {
        this.d2WordChecker = d2WordChecker;
    }

    public String isD2Word(String problem) {
        return d2WordChecker.check(problem);
    }
}

class D2WordChecker {

    public static final String D2_REGEX = ".*(D2|d2).*";
    public static final String D2_WORD = "D2";
    public static final String NOT_D2_WORD = "unrated";

    public String check(String problem) {
        return problem.matches(D2_REGEX) ? D2_WORD : NOT_D2_WORD;
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

import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideProblems")
    @DisplayName("d2 혹은 D2가 포함된 문제라면 D2를 반환하고 아니라면 unrated를 반환한다.")
    void isD2ProblemTest(String problem, String expected) {
        D2Word sut = new D2Word(new D2WordChecker());

        String actual = sut.isD2Word(problem);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideProblems() {
        return Stream.of(
                Arguments.of("abc", "unrated"),
                Arguments.of("D2", "D2"),
                Arguments.of(" D2 ", "D2"),
                Arguments.of(" D 2 ", "unrated"),
                Arguments.of(" D 2  d2", "D2"),
                Arguments.of("naver d2", "D2"),
                Arguments.of("naver d3", "unrated")
        );
    }
}

class D2Word {

    private final D2WordChecker d2WordChecker;

    public D2Word(D2WordChecker d2WordChecker) {
        this.d2WordChecker = d2WordChecker;
    }

    public String isD2Word(String problem) {
        return d2WordChecker.check(problem);
    }
}

class D2WordChecker {

    public static final String D2_REGEX = ".*(D2|d2).*";
    public static final String D2_WORD = "D2";
    public static final String NOT_D2_WORD = "unrated";

    public String check(String problem) {
        return problem.matches(D2_REGEX) ? D2_WORD : NOT_D2_WORD;
    }
}
~~~
