# XORChic (Bronze 2)

### 문제 설명

범기가 영수의 파일에 XOR 연산을 수행해버렸다!

영수의 파일은 문자열 S를 담고 있었고, 문자열 S는 알파벳 대문자로만 구성되어 있었다. 파일에 XOR 연산을 수행하면, 파일이 담고 있는 문자열의 모든 문자에 대해서 XOR 연산이 수행되어 파일 내용이 변경된다.

XOR 연산을 사용하려면, key가 필요하다. S = "ABCD", key = 10인 경우라면, XOR 연산을 수행한 문자열은 "KHIN"이 되어버린다. "A"의 아스키 코드는 65이고, 여기에 10과 XOR 연산을 수행하면 결과는 75가 된다. 아스키 코드로 75는 "K"이기 때문에, "A"는 "K"로 암호화 되고, 나머지 문자도 이와 같은 방식을 사용한다.

영수는 파일을 복호화하려고 하지만, 사용된 key의 값을 모르고 있다. 파일의 처음 8글자가 "CHICKENS"로 항상 시작한다는 정보를 알고 있을 때, 원래 파일로 복호화할 수 있을까?

출처 : https://www.acmicpc.net/problem/17285

---

#### 풀이
~~~java
import java.util.Scanner;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        XorPassword xorPassword = new XorPassword(scanner.nextLine());
        System.out.println(xorPassword.decode());
    }
}

class XorPassword {

    private static final char FIRST_PASSWORD_KEY = 'C';
    private static final int FIRST_PASSWORD_INDEX = 0;

    private final String password;

    public XorPassword(String password) {
        this.password = password;
    }

    public String decode() {
        return password.chars()
                .mapToObj(this::decodePassword)
                .collect(Collectors.joining());
    }

    private String decodePassword(int c) {
        return String.valueOf((char)(c ^ getKey(password)));
    }

    private int getKey(String password) {
        return FIRST_PASSWORD_KEY ^ password.charAt(FIRST_PASSWORD_INDEX);
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
    @MethodSource("providePassword")
    @DisplayName("암호를 해독한다.")
    void xorPasswordDecodesPassword(String password, String expected) {
        XorPassword sut = new XorPassword(password);

        String actual = sut.decode();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> providePassword() {
        return Stream.of(
                Arguments.of("BIHBJDOR", "CHICKENS"),
                Arguments.of("BIHBJDOR@C", "CHICKENSAB"),
                Arguments.of("U^_U]SXEPY", "CHICKENSFO"),
                Arguments.of("U^_U]SXEPYDS@SDOB^_XQ", "CHICKENSFOREVERYTHING")
        );
    }
}

class XorPassword {

    private static final char FIRST_PASSWORD_KEY = 'C';
    private static final int FIRST_PASSWORD_INDEX = 0;

    private final String password;

    public XorPassword(String password) {
        this.password = password;
    }

    public String decode() {
        return password.chars()
                .mapToObj(this::decodePassword)
                .collect(Collectors.joining());
    }

    private String decodePassword(int c) {
        return String.valueOf((char)(c ^ getKey(password)));
    }

    private int getKey(String password) {
        return FIRST_PASSWORD_KEY ^ password.charAt(FIRST_PASSWORD_INDEX);
    }
}
~~~
