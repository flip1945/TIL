# 연길이의 이상형 (Bronze 3)

### 문제 설명

졸업을 앞둔 연길이는 크리스마스가 다가올수록 외로움을 느낀다.

그런 연길이를 위해 동우는 소개팅을 시켜주지는 않고 연길이의 이상향을 찾는 것을 도와주고자 한다.

MBTI 신봉자인 연길이는 자신과 정반대인 사람에게 매력을 느낀다. 즉, MBTI의 네가지 지표가 모두 자신과 반대인 사람이 연길이의 이상형이다.

MBTI는 다음과 같은 네 가지 척도로 성격을 표시한다. 각각의 척도는 두 가지 극이 되는 성격으로 이루어져 있다.

|지표	|설명|
|-|-|
|외향(Extroversion)/	내향(Introversion)	|선호하는 세계:세상과 타인 / 내면 세계|
|감각(Sensation)/	직관(iNtuition)	|인식형태: 실제적인 인식/ 실제 너머로 인식|
|사고(Thinking)/	감정(Feeling)	|판단기준: 사실과 진실 위주 / 관계와 사람 위주|
|판단(Judging)/	인식(Perceiving)	|생활양식: 계획적인 생활 / 즉흥적인 생활|

네 가지 척도마다 두 가지 경우가 존재하므로, 총 16가지의 유형이 만들어진다. 유형은 각 경우를 나타내는 알파벳 한 글자씩을 따서 네 글자로 표시한다. 다음은 MBTI의 유형들이다.

|구분|	감각/사고|	감각/감정|	직관/감정|	직관/사고|
|-|-|-|-|-|
|내향/판단|	ISTJ|	ISFJ|	INFJ|	INTJ|
|내향/인식|	ISTP|	ISFP|	INFP|	INTP|
|외향/인식|	ESTP|	ESFP|	ENFP|	ENTP|
|외향/판단|	ESTJ|	ESFJ|	ENFJ|	ENTJ|

연길이가 자신의 이상향을 무사히 찾을 수 있도록 도와주자!

출처 : https://www.acmicpc.net/problem/20540

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        MBTI mbti = MBTI.from(scanner.nextLine());
        System.out.println(mbti.contrary());
    }
}

class MBTI {

    public static final String DELIMITER = "";
    public static final int ENERGY_INDEX = 0;
    public static final int INFORMATION_INDEX = 1;
    public static final int DECISIONS_INDEX = 2;
    public static final int LIFE_STYLE_INDEX = 3;

    private final Energy energy;
    private final Information information;
    private final Decisions decisions;
    private final LifeStyle lifestyle;

    public MBTI(Energy energy, Information information, Decisions decisions, LifeStyle lifestyle) {
        this.energy = energy;
        this.information = information;
        this.decisions = decisions;
        this.lifestyle = lifestyle;
    }

    public static MBTI from(String mbti) {
        String[] split = mbti.split(DELIMITER);
        return new MBTI(Energy.valueOf(split[ENERGY_INDEX]), Information.valueOf(split[INFORMATION_INDEX]),
                Decisions.valueOf(split[DECISIONS_INDEX]), LifeStyle.valueOf(split[LIFE_STYLE_INDEX]));
    }

    public MBTI contrary() {
        return new MBTI(energy.opposite(), information.opposite(), decisions.opposite(), lifestyle.opposite());
    }

    @Override
    public String toString() {
        return energy.name() + information.name() + decisions.name() + lifestyle.name();
    }

    enum Energy {
        I, E;

        public Energy opposite() {
            return this.equals(E) ? I : E;
        }
    }

    enum Information {
        S, N;

        public Information opposite() {
            return this.equals(N) ? S : N;
        }
    }

    enum Decisions {
        T, F;

        public Decisions opposite() {
            return this.equals(F) ? T : F;
        }
    }

    enum LifeStyle {
        J, P;

        public LifeStyle opposite() {
            return this.equals(P) ? J : P;
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

import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideMBTIs")
    @DisplayName("정반대의 MBTI를 반환한다.")
    void contraryMBTITest(String mbti, String expected) {
        MBTI sut = MBTI.from(mbti);

        String actual = sut.contrary().toString();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideMBTIs() {
        return Stream.of(
                Arguments.of("ISTJ", "ENFP"),
                Arguments.of("ENFP", "ISTJ"),
                Arguments.of("ISTP", "ENFJ"),
                Arguments.of("ESTJ", "INFP"),
                Arguments.of("INFP", "ESTJ")
        );
    }
}

class MBTI {

    public static final String DELIMITER = "";
    public static final int ENERGY_INDEX = 0;
    public static final int INFORMATION_INDEX = 1;
    public static final int DECISIONS_INDEX = 2;
    public static final int LIFE_STYLE_INDEX = 3;

    private final Energy energy;
    private final Information information;
    private final Decisions decisions;
    private final LifeStyle lifestyle;

    public MBTI(Energy energy, Information information, Decisions decisions, LifeStyle lifestyle) {
        this.energy = energy;
        this.information = information;
        this.decisions = decisions;
        this.lifestyle = lifestyle;
    }

    public static MBTI from(String mbti) {
        String[] split = mbti.split(DELIMITER);
        return new MBTI(Energy.valueOf(split[ENERGY_INDEX]), Information.valueOf(split[INFORMATION_INDEX]),
                Decisions.valueOf(split[DECISIONS_INDEX]), LifeStyle.valueOf(split[LIFE_STYLE_INDEX]));
    }

    public MBTI contrary() {
        return new MBTI(energy.opposite(), information.opposite(), decisions.opposite(), lifestyle.opposite());
    }

    @Override
    public String toString() {
        return energy.name() + information.name() + decisions.name() + lifestyle.name();
    }

    enum Energy {
        I, E;

        public Energy opposite() {
            return this.equals(E) ? I : E;
        }
    }

    enum Information {
        S, N;

        public Information opposite() {
            return this.equals(N) ? S : N;
        }
    }

    enum Decisions {
        T, F;

        public Decisions opposite() {
            return this.equals(F) ? T : F;
        }
    }

    enum LifeStyle {
        J, P;

        public LifeStyle opposite() {
            return this.equals(P) ? J : P;
        }
    }
}
~~~
