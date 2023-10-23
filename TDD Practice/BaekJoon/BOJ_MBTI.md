# MBTI (Bronze 4)

### 문제 설명

진호는 요즘 유행하는 심리 검사인 MBTI에 관심이 많다. MBTI는 아래와 같이 네 가지 척도로 사람들의 성격을 구분해서, 총 $16$가지의 유형중에서 자신의 유형을 찾을 수 있는 심리 검사이다.

* 내향(I) / 외향(E)
* 직관(N) / 감각(S)
* 감정(F) / 사고(T)
* 인식(P) / 판단(J)

모든 유형의 목록은 다음과 같다.

* INFP, ENFP, ISFP, ESFP, INTP, ENTP, ISTP, ESTP, INFJ, ENFJ, ISFJ, ESFJ, INTJ, ENTJ, ISTJ, ESTJ

진호는 $N$명의 친구들에게 MBTI 유형을 물어 봤다. 이 중에서 진호와 MBTI 유형이 같은 사람의 수는 얼마일까?

출처 : https://www.acmicpc.net/problem/25640

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String mBTI = scanner.nextLine();
        int n = Integer.parseInt(scanner.nextLine());

        MBTIs mbtIs = new MBTIs(inputMBTIs(scanner, n));
        System.out.println(mbtIs.sameMBTI(mBTI));
    }

    private static List<String> inputMBTIs(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class MBTIs {

    private final List<String> mBTIs;

    public MBTIs(List<String> mBTIs) {
        this.mBTIs = mBTIs;
    }

    public int sameMBTI(String mBTI) {
        return (int) mBTIs.stream()
                .filter(mbti -> mbti.equals(mBTI))
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

import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideMBTIs")
    @DisplayName("본인의 MBTI와 유형이 같은 사람의 수를 반환한다.")
    void mBTIsReturnsSameMBTICounts(String mBTI, List<String> mBTIs, int expected) {
        MBTIs sut = new MBTIs(mBTIs);

        int actual = sut.sameMBTI(mBTI);

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideMBTIs() {
        return Stream.of(
                Arguments.of("ISTJ", List.of("ISTJ"), 1),
                Arguments.of("ISTJ", List.of("ISTJ", "ISTJ"), 2),
                Arguments.of("ISTJ", List.of("ISTJ", "ENTP", "ISTJ"), 2),
                Arguments.of("ESTJ", List.of("ISTP", "ESTJ", "INTP", "ESTJ", "ENTJ"), 2),
                Arguments.of("INTP", List.of("INTP", "INTP", "ESFP", "ISFP", "INFP", "INTP"), 3)
        );
    }
}

class MBTIs {

    private final List<String> mBTIs;

    public MBTIs(List<String> mBTIs) {
        this.mBTIs = mBTIs;
    }

    public int sameMBTI(String mBTI) {
        return (int) mBTIs.stream()
                .filter(mbti -> mbti.equals(mBTI))
                .count();
    }
}
~~~
