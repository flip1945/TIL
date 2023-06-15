# CTP공국으로 이민 가자 (Bronze 2)

### 문제 설명

신생국가 CTP공국은 자신들만의 글자가 없다. CTP공국의 왕 준형이는 전 세계 표준 언어인 알파벳을 사용하기로 했다. 하지만 숫자에 미친 사람들이 모인 CTP공국 주민들은 알파벳을 사용할 때 평범한 알파벳이 아니라 쓰려고 하는 알파벳이 앞에서부터 몇 번째 알파벳인지를 의미하는 숫자로 나타낸다. 예를 들어 ‘A’는 ‘1’로, ‘Z’는 ‘26’로 나타낸다.

CTP공국은 현재 부흥 중이라 새로 국민이 되고자 하는 사람이 많다. 하지만 아무나 CTP공국의 국민이 될 수는 없는 법. CTP공국의 이민국장 인덕이는 이민 신청자들이 CTP 공국의 글자체계를 잘 알고 있는지 확인하는 시험문제를 내기로 했다.

시험문제는 두 가지 종류로 구분된다. CTP공국의 글자가 주어졌을 때 알파벳을 쓰는 문제와 알파벳이 주어졌을 때 CTP공국의 글자를 쓰는 문제 두 가지이다.

너무 많은 이민 신청자들 때문에 시험문제 채점에 골치가 아픈 인덕이를 위해 주어진 시험문제의 정답을 알려주는 프로그램을 작성하라.

출처 : https://www.acmicpc.net/problem/12778

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < t; i++) {
            StringTokenizer st = new StringTokenizer(scanner.nextLine());
            int m = Integer.parseInt(st.nextToken());
            String type = st.nextToken();
            String letters = scanner.nextLine();
            Ctp ctp = new Ctp(type, letters);
            System.out.println(ctp.convert());
        }
    }
}

class Ctp {

    private final Type type;
    private final String letters;

    public Ctp(String type, String letters) {
        this.type = Type.valueOf(type);
        this.letters = letters;
    }

    public String convert() {
        return String.join(" ", convertToStrings(letters));
    }

    private List<String> convertToStrings(String letters) {
        return Arrays.stream(letters.split(" "))
                .map(type::convert)
                .collect(Collectors.toList());
    }

    enum Type {
        C(letter -> String.valueOf(letter.charAt(0) - 'A' + 1)),
        N(letter -> String.valueOf((char)(Integer.parseInt(letter) + 'A' - 1)));

        private final Function<String, String> convertFunction;

        Type(Function<String, String> convertFunction) {
            this.convertFunction = convertFunction;
        }

        public String convert(String letter) {
            return convertFunction.apply(letter);
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

import java.util.Arrays;
import java.util.List;
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideAlphabets")
    @DisplayName("알파벳을 CTP 문자로 변환한다.")
    void convertToCtpTest(String type, String letters, String expected) {
        Ctp sut = new Ctp(type, letters);

        String actual = sut.convert();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideAlphabets() {
        return Stream.of(
                Arguments.of("C", "A", "1"),
                Arguments.of("C", "B", "2"),
                Arguments.of("C", "Z", "26"),
                Arguments.of("C", "A B", "1 2"),
                Arguments.of("C", "C T P", "3 20 16"),
                Arguments.of("C", "Z Z Z Z", "26 26 26 26")
        );
    }

    @ParameterizedTest
    @MethodSource("provideCtp")
    @DisplayName("CTP 문자를 알파벳으로 변환한다.")
    void convertToAlphabetTest(String type, String letters, String expected) {
        Ctp sut = new Ctp(type, letters);

        String actual = sut.convert();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCtp() {
        return Stream.of(
                Arguments.of("N", "1", "A"),
                Arguments.of("N", "2", "B"),
                Arguments.of("N", "1 2", "A B"),
                Arguments.of("N", "9 14 8 1", "I N H A")
        );
    }
}

class Ctp {

    private final Type type;
    private final String letters;

    public Ctp(String type, String letters) {
        this.type = Type.valueOf(type);
        this.letters = letters;
    }

    public String convert() {
        return String.join(" ", convertToStrings(letters));
    }

    private List<String> convertToStrings(String letters) {
        return Arrays.stream(letters.split(" "))
                .map(type::convert)
                .collect(Collectors.toList());
    }

    enum Type {
        C(letter -> String.valueOf(letter.charAt(0) - 'A' + 1)),
        N(letter -> String.valueOf((char)(Integer.parseInt(letter) + 'A' - 1)));

        private final Function<String, String> convertFunction;

        Type(Function<String, String> convertFunction) {
            this.convertFunction = convertFunction;
        }

        public String convert(String letter) {
            return convertFunction.apply(letter);
        }
    }
}
~~~
