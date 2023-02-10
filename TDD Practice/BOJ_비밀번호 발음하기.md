# 비밀번호 발음하기 (Silver 5)

### 문제 설명

좋은 패스워드를 만드는것은 어려운 일이다. 대부분의 사용자들은 buddy처럼 발음하기 좋고 기억하기 쉬운 패스워드를 원하나, 이런 패스워드들은 보안의 문제가 발생한다. 어떤 사이트들은 xvtpzyo 같은 비밀번호를 무작위로 부여해 주기도 하지만, 사용자들은 이를 외우는데 어려움을 느끼고 심지어는 포스트잇에 적어 컴퓨터에 붙여놓는다. 가장 이상적인 해결법은 '발음이 가능한' 패스워드를 만드는 것으로 적당히 외우기 쉬우면서도 안전하게 계정을 지킬 수 있다. 

회사 FnordCom은 그런 패스워드 생성기를 만들려고 계획중이다. 당신은 그 회사 품질 관리 부서의 직원으로 생성기를 테스트해보고 생성되는 패스워드의 품질을 평가하여야 한다. 높은 품질을 가진 비밀번호의 조건은 다음과 같다.

1. 모음(a,e,i,o,u) 하나를 반드시 포함하여야 한다.
2. 모음이 3개 혹은 자음이 3개 연속으로 오면 안 된다.
3. 같은 글자가 연속적으로 두번 오면 안되나, ee 와 oo는 허용한다.

이 규칙은 완벽하지 않다;우리에게 친숙하거나 발음이 쉬운 단어 중에서도 품질이 낮게 평가되는 경우가 많이 있다.

출처 : https://www.acmicpc.net/problem/4659

---

#### 풀이
~~~java
import java.util.*;
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        PasswordChecker passwordChecker = new PasswordChecker();
        String input;
        while (!(input = bf.readLine()).equals("end")) {
            System.out.println(passwordChecker.check(input));
        }
    }
}

class PasswordChecker {
    private static final Set<Character> VOWELS = Set.of('a', 'e', 'i', 'o', 'u');
    private static final int DEFAULT_STREAK_OF_VOWELS_OR_CONSONANTS = 1;
    private int streakOfVowelsOrConsonants;
    private boolean hasVowelInPassword;

    public String check(String password) {
        streakOfVowelsOrConsonants = 0;
        hasVowelInPassword = false;
        char prev = 0;

        for (char current : password.toCharArray()) {
            checkContainVowelInPassword(current);
            calculateStreakOfVowelsOrConsonants(prev, current);
            if (isNotAcceptablePassword(prev, current)) {
                return toResultString(password, false);
            }
            prev = current;
        }
        return toResultString(password, hasVowelInPassword);
    }

    private void checkContainVowelInPassword(char current) {
        if (isVowel(current)) {
            hasVowelInPassword = true;
        }
    }

    private boolean isVowel(char current) {
        return VOWELS.contains(current);
    }

    private void calculateStreakOfVowelsOrConsonants(char prev, char current) {
        if (isVowel(prev) == isVowel(current)) {
            streakOfVowelsOrConsonants++;
            return;
        }
        streakOfVowelsOrConsonants = DEFAULT_STREAK_OF_VOWELS_OR_CONSONANTS;
    }

    private boolean isNotAcceptablePassword(char prev, char current) {
        return streakOfVowelsOrConsonants > 2 || isSameCharacter(prev, current);
    }

    private boolean isSameCharacter(char prev, char current) {
        if (isExceptionCharacter(prev, current)) {
            return false;
        }
        return current == prev;
    }

    private boolean isExceptionCharacter(char prev, char current) {
        return (prev == 'e' && current == 'e') || (prev == 'o' && current == 'o');
    }

    private String toResultString(String password, boolean acceptable) {
        return "<" + password + "> is " + (acceptable ? "" : "not ") + "acceptable.";
    }
}

~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Set;

import static org.junit.jupiter.api.Assertions.*;

public class MainTest {

    @Test
    void passwordCheckTest() {
        PasswordChecker passwordChecker = new PasswordChecker();

        assertEquals("<a> is acceptable.", passwordChecker.check("a"));
        assertEquals("<e> is acceptable.", passwordChecker.check("e"));
        assertEquals("<b> is not acceptable.", passwordChecker.check("b"));
        assertEquals("<tv> is not acceptable.", passwordChecker.check("tv"));
        assertEquals("<ae> is acceptable.", passwordChecker.check("ae"));
        assertEquals("<aa> is not acceptable.", passwordChecker.check("aa"));
        assertEquals("<aea> is not acceptable.", passwordChecker.check("aea"));
        assertEquals("<atvf> is not acceptable.", passwordChecker.check("atvf"));
        assertEquals("<tee> is acceptable.", passwordChecker.check("tee"));
        assertEquals("<foo> is acceptable.", passwordChecker.check("foo"));

        assertEquals("<ptoui> is not acceptable.", passwordChecker.check("ptoui"));
        assertEquals("<bontres> is not acceptable.", passwordChecker.check("bontres"));
        assertEquals("<zoggax> is not acceptable.", passwordChecker.check("zoggax"));
        assertEquals("<wiinq> is not acceptable.", passwordChecker.check("wiinq"));
        assertEquals("<eep> is acceptable.", passwordChecker.check("eep"));
        assertEquals("<houctuh> is acceptable.", passwordChecker.check("houctuh"));

        assertEquals("<tba> is acceptable.", passwordChecker.check("tba"));
    }

    static class PasswordChecker {
        private static final Set<Character> VOWELS = Set.of('a', 'e', 'i', 'o', 'u');
        private static final int DEFAULT_STREAK_OF_VOWELS_OR_CONSONANTS = 1;
        private int streakOfVowelsOrConsonants;
        private boolean hasVowelInPassword;

        public String check(String password) {
            streakOfVowelsOrConsonants = 0;
            hasVowelInPassword = false;
            char prev = 0;

            for (char current : password.toCharArray()) {
                checkContainVowelInPassword(current);
                calculateStreakOfVowelsOrConsonants(prev, current);
                if (isNotAcceptablePassword(prev, current)) {
                    return toResultString(password, false);
                }
                prev = current;
            }
            return toResultString(password, hasVowelInPassword);
        }

        private void checkContainVowelInPassword(char current) {
            if (isVowel(current)) {
                hasVowelInPassword = true;
            }
        }

        private boolean isVowel(char current) {
            return VOWELS.contains(current);
        }

        private void calculateStreakOfVowelsOrConsonants(char prev, char current) {
            if (isVowel(prev) == isVowel(current)) {
                streakOfVowelsOrConsonants++;
                return;
            }
            streakOfVowelsOrConsonants = DEFAULT_STREAK_OF_VOWELS_OR_CONSONANTS;
        }

        private boolean isNotAcceptablePassword(char prev, char current) {
            return streakOfVowelsOrConsonants > 2 || isSameCharacter(prev, current);
        }

        private boolean isSameCharacter(char prev, char current) {
            if (isExceptionCharacter(prev, current)) {
                return false;
            }
            return current == prev;
        }

        private boolean isExceptionCharacter(char prev, char current) {
            return (prev == 'e' && current == 'e') || (prev == 'o' && current == 'o');
        }

        private String toResultString(String password, boolean acceptable) {
            return "<" + password + "> is " + (acceptable ? "" : "not ") + "acceptable.";
        }
    }
}
~~~
