# Send me the money (Bronze 1)

### 문제 설명

석규는 해외로 저렴하고 간편하게 송금할 수 있는 센트비 서비스를 이용하여 CTP 왕국에 놀러간 형동이에게 돈을 보내주려고 한다. 하지만 안타깝게도 석규는 센트비 비밀번호를 까먹어버렸고 돈을 보내주지 못한다. 

다행히도 석규는 평소에 포스트잇에 비밀번호를 적어놓는다. 비밀번호는 알파벳 대문자로만 구성이 되어있으며 석규는 이 중 일부를 정확히 기억하고 있다.

석규는 포스트잇을 확인하여 비밀번호를 입력하려고 했지만, 포스트잇은 여러 장 존재했고 이 중 어떤 포스트잇이 센트비 비밀번호가 적힌 포스트잇인지 모른다.

석규는 센트비 비밀번호의 알파벳 중 등장하는 순서대로 N글자만 정확히 기억하고 있으며 포스트잇 중에 이 순서를 갖는 포스트잇이 센트비 비밀번호일 가능성이 있는 포스트잇이다. 

예를 들어, 석규가 ABB를 기억한다면 BBAB이 적힌 포스트잇은 비밀번호일 가능성이 없고, HAEBBC가 적힌 포스트잇은 비밀번호일 가능성이 있다. 

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/15786/1.png" width=328>

석규는 형동이에게 송금해주기 위해 포스트잇들 중 비밀번호가 적힌 포스트잇일 가능성이 있는 포스트잇들을 따로 분류하려고 한다. 석규가 기억하는 알파벳 N글자와 포스트잇 M개가 주어질 때, 해당 포스트잇에 적힌 알파벳이 비밀번호일 가능성이 있는지 여부를 판단하여 보자.

출처 : https://www.acmicpc.net/problem/15786

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        Password password = new Password(scanner.nextLine());
        for (int i = 0; i < m; i++) {
            System.out.println(password.isPossible(scanner.nextLine()));
        }
    }
}

class Password {

    private final String partialPassword;

    public Password(String partialPassword) {
        this.partialPassword = partialPassword;
    }

    public boolean isPossible(String passwordCandidate) {
        int index = 0;
        for (char letter : passwordCandidate.toCharArray()) {
            index = increaseIndexIfPartOfPassword(index, letter);
        }
        return index == partialPassword.length();
    }

    private int increaseIndexIfPartOfPassword(int index, char letter) {
        if (isPartOfPassword(index, letter)) {
            return index + 1;
        }
        return index;
    }

    private boolean isPartOfPassword(int index, char letter) {
        return index < partialPassword.length() && partialPassword.charAt(index) == letter;
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
    @MethodSource("provideCandidates")
    @DisplayName("일부 패스워드가 있을 때 패스워드 후보가 패스워드가 될 수 있는 지 확인한다.")
    void isPossiblePasswordTest(String partialPassword, String passwordCandidate, boolean expected) {
        Password sut = new Password(partialPassword);

        boolean actual = sut.isPossible(passwordCandidate);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCandidates() {
        return Stream.of(
                Arguments.of("A", "A", true),
                Arguments.of("A", "AB", true),
                Arguments.of("A", "CAB", true),
                Arguments.of("AB", "CABD", true),
                Arguments.of("AD", "CABD", true),
                Arguments.of("AD", "A", false),
                Arguments.of("PPAP", "PPPPA", false),
                Arguments.of("PPAP", "APPPP", false),
                Arguments.of("PPAP", "PPPAP", true),
                Arguments.of("PPAP", "PPPAP", true),
                Arguments.of("CTP", "P", false),
                Arguments.of("CTP", "CHALLENGETHEPROGRAMING", true)
        );
    }
}

class Password {

    private final String partialPassword;

    public Password(String partialPassword) {
        this.partialPassword = partialPassword;
    }

    public boolean isPossible(String passwordCandidate) {
        int index = 0;
        for (char letter : passwordCandidate.toCharArray()) {
            index = increaseIndexIfPartOfPassword(index, letter);
        }
        return index == partialPassword.length();
    }

    private int increaseIndexIfPartOfPassword(int index, char letter) {
        if (isPartOfPassword(index, letter)) {
            return index + 1;
        }
        return index;
    }

    private boolean isPartOfPassword(int index, char letter) {
        return index < partialPassword.length() && partialPassword.charAt(index) == letter;
    }
}
~~~
