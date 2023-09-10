# 성택이의 은밀한 비밀번호 (Bronze 5)

### 문제 설명

부산사이버대학교 학생 성택이는 엄마의 의뢰를 받아 주어진 문자열이 현관문 비밀번호에 사용 가능한지 알아내야 한다. 성택이는 공부해야 하므로 우리가 도와주자!

사용할 수 있는 비밀번호의 규칙은 다음과 같다.

1. **비밀번호는 6자리 이상 9자리 이하여야 한다.**

예를 들어, 123124는 올바른 비밀번호이고, 1202727161은 잘못된 비밀번호이다. 문자열이 주어졌을 때 현관문 비밀번호로 사용할 수 있는지 판단하자.

출처 : https://www.acmicpc.net/problem/25372

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            Password password = new Password(scanner.nextLine());
            System.out.println(password.isValid());
        }
    }
}

class Password {

    private static final String VALID_PASSWORD = "yes";
    private static final String INVALID_PASSWORD = "no";
    private static final int MIN_VALID_PASSWORD_LENGTH = 6;
    private static final int MAX_VALID_PASSWORD_LENGTH = 9;

    private final String password;

    public Password(String password) {
        this.password = password;
    }

    public String isValid() {
        return isValidPassword() ? VALID_PASSWORD : INVALID_PASSWORD;
    }

    private boolean isValidPassword() {
        return password.length() >= MIN_VALID_PASSWORD_LENGTH && password.length() <= MAX_VALID_PASSWORD_LENGTH;
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
    @MethodSource("provideValidPassword")
    @DisplayName("유효한 비밀번호인지 확인해야 한다.")
    void shouldCheckValidPassword(String password) {
        Password sut = new Password(password);

        String actual = sut.isValid();

        assertEquals("yes", actual);
    }

    private static Stream<Arguments> provideValidPassword() {
        return Stream.of(
                Arguments.of("123456"),
                Arguments.of("1234567"),
                Arguments.of("12345678"),
                Arguments.of("123456789"),
                Arguments.of("password"),
                Arguments.of("PASSWORD")
        );
    }

    @ParameterizedTest
    @MethodSource("provideInvalidPassword")
    @DisplayName("유효하지 않은 비밀번호인지 확인해야 한다.")
    void shouldCheckInvalidPassword(String password) {
        Password sut = new Password(password);

        String actual = sut.isValid();

        assertEquals("no", actual);
    }

    private static Stream<Arguments> provideInvalidPassword() {
        return Stream.of(
                Arguments.of("1234"),
                Arguments.of("1234567890"),
                Arguments.of("0"),
                Arguments.of("a"),
                Arguments.of("A"),
                Arguments.of("abcdefghijk"),
                Arguments.of("one1two2three3")
        );
    }
}

class Password {

    private static final String VALID_PASSWORD = "yes";
    private static final String INVALID_PASSWORD = "no";
    private static final int MIN_VALID_PASSWORD_LENGTH = 6;
    private static final int MAX_VALID_PASSWORD_LENGTH = 9;

    private final String password;

    public Password(String password) {
        this.password = password;
    }

    public String isValid() {
        return isValidPassword() ? VALID_PASSWORD : INVALID_PASSWORD;
    }

    private boolean isValidPassword() {
        return password.length() >= MIN_VALID_PASSWORD_LENGTH && password.length() <= MAX_VALID_PASSWORD_LENGTH;
    }
}
~~~
