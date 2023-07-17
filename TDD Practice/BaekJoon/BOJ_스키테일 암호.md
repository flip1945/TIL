# 스키테일 암호 (Bronze 3)

### 문제 설명

고대 그리스의 옛 나라인 스파르타의 군대에서는 비밀메시지를 전하는 방법으로 스키테일 암호를 사용했다.

스키테일 암호는 스키테일(Scytale)이라고 하는 정해진 굵기의 원통형 막대에 종이로 된 리본을 위에서 아래로 감은 다음 옆으로 메시지를 적는 방식으로 메세지를 암호화한다. 리본을 풀어 길게 늘어선 글을 읽으면 무슨 뜻인지 전혀 알 수 없지만, 암호화할 때와 같은 굵기의 막대에 감으면 내용을 알 수 있게 된다.

다음은 굵기 3의 막대를 사용하여 "iupc" 라는 문자열을 암호화하는 예시이다.

<img src='https://upload.acmicpc.net/21dd22ef-83b1-4ad7-9fec-d645306ee12f/-/preview/' width='400px'>

굵기가 
\(X\)인 막대에 리본을 감고 세로로 글자 
\(X\)개를 적으면 막대를 한바퀴 돌아오게 된다. 이 막대는 굵기가 3이므로, 세로로 3글자를 적으면 막대를 한바퀴 돌아올 것이다.

암호화하는 문자열을 리본의 가장 왼쪽 끝 부분을 포함하는 가로 한 줄만 사용하여 쓰고, 남은 공간은 아무 문자로나 채운다.

<img src='https://upload.acmicpc.net/b6ade534-0048-480f-95aa-d7ce528f2c1d/-/preview/' width='400px'>

마지막으로 막대에서 리본을 풀면 암호화가 완료된다.

<img src='https://upload.acmicpc.net/000d5186-c0c5-4847-bde5-0ce9b055e2d6/-/preview/' width='786px'>

스키테일 암호로 암호화한 문자열과 막대의 굵기가 주어진다. 암호를 해독해 보자!

출처 : https://www.acmicpc.net/problem/23080

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int k = Integer.parseInt(scanner.nextLine());
        ScytalePassword scytalePassword = new ScytalePassword(k, scanner.nextLine());

        System.out.println(scytalePassword.decode());
    }
}

class ScytalePassword {

    private final int thickness;
    private final String scytalePassword;

    public ScytalePassword(int thickness, String scytalePassword) {
        this.thickness = thickness;
        this.scytalePassword = scytalePassword;
    }

    public String decode() {
        return IntStream.range(0, scytalePassword.length())
                .filter(this::canDecode)
                .mapToObj(this::getStringOfIndex)
                .collect(Collectors.joining());
    }

    private String getStringOfIndex(int i) {
        return String.valueOf(scytalePassword.charAt(i));
    }

    private boolean canDecode(int i) {
        return i % thickness == 0;
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
    @MethodSource("provideScytalePasswords")
    @DisplayName("스키테일 암호를 복호화한다.")
    void scytalePasswordTest(int thickness, String scytalePassword, String expected) {
        ScytalePassword sut = new ScytalePassword(thickness, scytalePassword);

        String actual = sut.decode();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideScytalePasswords() {
        return Stream.of(
                Arguments.of(3, "abcd", "ad"),
                Arguments.of(3, "abcdefg", "adg"),
                Arguments.of(4, "abcdefg", "ae"),
                Arguments.of(3, "a", "a"),
                Arguments.of(10, "a", "a")
        );
    }
}

class ScytalePassword {

    private final int thickness;
    private final String scytalePassword;

    public ScytalePassword(int thickness, String scytalePassword) {
        this.thickness = thickness;
        this.scytalePassword = scytalePassword;
    }

    public String decode() {
        return IntStream.range(0, scytalePassword.length())
                .filter(this::canDecode)
                .mapToObj(this::getStringOfIndex)
                .collect(Collectors.joining());
    }

    private String getStringOfIndex(int i) {
        return String.valueOf(scytalePassword.charAt(i));
    }

    private boolean canDecode(int i) {
        return i % thickness == 0;
    }
}
~~~
