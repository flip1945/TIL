# 민균이의 비밀번호 (Bronze 1)

### 문제 설명

창영이는 민균이의 컴퓨터를 해킹해 텍스트 파일 하나를 자신의 메일로 전송했다. 파일에는 단어가 한 줄에 하나씩 적혀있었고, 이 중 하나는 민균이가 온라인 저지에서 사용하는 비밀번호이다.

파일을 살펴보던 창영이는 모든 단어의 길이가 홀수라는 사실을 알아내었다. 그리고 언젠가 민균이가 이 목록에 대해서 얘기했던 것을 생각해냈다. 민균이의 비밀번호는 목록에 포함되어 있으며, 비밀번호를 뒤집어서 쓴 문자열도 포함되어 있다.

예를 들어, 민균이의 비밀번호가 "tulipan"인 경우에 목록에는 "napilut"도 존재해야 한다. 알 수 없는 이유에 의해 모두 비밀번호로 사용 가능하다고 한다.

민균이의 파일에 적혀있는 단어가 모두 주어졌을 때, 비밀번호의 길이와 가운데 글자를 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/9933

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        Set<String> passwordCandidates = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toSet());

        String rightPassword = getRightPassword(passwordCandidates);
        System.out.println(rightPassword.length() + " " + getMiddleWordOfRightPassword(rightPassword));
    }

    static String getMiddleWordOfRightPassword(String rightPassword) {
        return String.valueOf(rightPassword.charAt(rightPassword.length() / 2));
    }

    static String getRightPassword(Set<String> passwordCandidates) {
        return passwordCandidates.stream()
                .filter(passwordCandidate -> isRightPassword(passwordCandidates, passwordCandidate))
                .findFirst()
                .orElse("");
    }

    static boolean isRightPassword(Set<String> passwordCandidates, String passwordCandidate) {
        return passwordCandidates.contains(reverseString(passwordCandidate));
    }

    static String reverseString(String candidate) {
        return new StringBuilder(candidate).reverse().toString();
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

import java.util.Set;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePasswordCandidatesAndLengths")
    @DisplayName("올바른 비밀번호의 길이를 출력한다.")
    void getRightPasswordLengthTest(Set<String> passwordCandidates, int expected) {
        String actual = getRightPassword(passwordCandidates);

        assertEquals(expected, actual.length());
    }

    private static Stream<Arguments> providePasswordCandidatesAndLengths() {
        return Stream.of(
                Arguments.of(Set.of("las", "god", "psala", "sal"), 3)
                , Arguments.of(Set.of("abc", "bbc", "asdf", "cbb"), 3)
                , Arguments.of(Set.of("kisik", "ptq", "tttrp", "tulipan"), 5)
                , Arguments.of(Set.of("abababa", "asdf", "string", "contains"), 7)
        );
    }

    @ParameterizedTest
    @MethodSource("providePasswordCandidates")
    @DisplayName("올바른 비밀번호의 가운데 글자를 출력한다.")
    void getRightPasswordTest(Set<String> passwordCandidates, String expected) {
        String actual = getMiddleWordOfRightPassword(getRightPassword(passwordCandidates));

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> providePasswordCandidates() {
        return Stream.of(
                Arguments.of(Set.of("las", "god", "psala", "sal"), "a")
                , Arguments.of(Set.of("abc", "bbc", "asdf", "cbb"), "b")
                , Arguments.of(Set.of("kisik", "ptq", "tttrp", "tulipan"), "s")
                , Arguments.of(Set.of("abababa", "asdf", "string", "contains"), "b")
        );
    }

    private String getMiddleWordOfRightPassword(String rightPassword) {
        return String.valueOf(rightPassword.charAt(rightPassword.length() / 2));
    }

    private String getRightPassword(Set<String> passwordCandidates) {
        return passwordCandidates.stream()
                .filter(passwordCandidate -> isRightPassword(passwordCandidates, passwordCandidate))
                .findFirst()
                .orElse("");
    }

    private boolean isRightPassword(Set<String> passwordCandidates, String passwordCandidate) {
        return passwordCandidates.contains(reverseString(passwordCandidate));
    }

    private String reverseString(String candidate) {
        return new StringBuilder(candidate).reverse().toString();
    }
}
~~~
