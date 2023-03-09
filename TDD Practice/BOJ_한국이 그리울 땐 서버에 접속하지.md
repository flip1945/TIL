# 한국이 그리울 땐 서버에 접속하지 (Silver 3)

### 문제 설명

선영이는 이번 학기에 오스트레일리아로 교환 학생을 가게 되었다. 

호주에 도착하고 처음 며칠은 한국 생각을 잊으면서 즐겁게 지냈다. 몇 주가 지나니 한국이 그리워지기 시작했다. 

선영이는 한국에 두고온 서버에 접속해서 디렉토리 안에 들어있는 파일 이름을 보면서 그리움을 잊기로 했다. 매일 밤, 파일 이름을 보면서 파일 하나하나에 얽힌 사연을 기억하면서 한국을 생각하고 있었다.

어느 날이었다. 한국에 있는 서버가 망가졌고, 그 결과 특정 패턴과 일치하는 파일 이름을 적절히 출력하지 못하는 버그가 생겼다.

패턴은 알파벳 소문자 여러 개와 별표(*) 하나로 이루어진 문자열이다.

파일 이름이 패턴에 일치하려면, 패턴에 있는 별표를 알파벳 소문자로 이루어진 임의의 문자열로 변환해 파일 이름과 같게 만들 수 있어야 한다. 별표는 빈 문자열로 바꿀 수도 있다. 예를 들어, "abcd", "ad", "anestonestod"는 모두 패턴 "a*d"와 일치한다. 하지만, "bcd"는 일치하지 않는다.

패턴과 파일 이름이 모두 주어졌을 때, 각각의 파일 이름이 패턴과 일치하는지 아닌지를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/9996

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        String pattern = scanner.nextLine();
        List<String> fileNames = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());

        fileNames.stream()
                .map(filename -> matchPattern(pattern, filename))
                .forEach(System.out::println);
    }

    static String matchPattern(String pattern, String string) {
        String[] patterns = pattern.split("\\*");
        if (canMatch(string, patterns)) {
            return "DA";
        }
        return "NE";
    }

    static boolean canMatch(String string, String[] patterns) {
        if (string.length() < patterns[0].length() + patterns[1].length()) {
            return false;
        }
        return string.startsWith(patterns[0]) && string.endsWith(patterns[1]);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.*;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void matchPatternTest() {
        assertEquals("DA", matchPattern("a*d", "abcd"));
        assertEquals("DA", matchPattern("a*d", "abbbcd"));
        assertEquals("NE", matchPattern("a*d", "abbb"));
        assertEquals("DA", matchPattern("a*d", "ad"));
        assertEquals("DA", matchPattern("a*d", "anestonestod"));
        assertEquals("NE", matchPattern("a*d", "facebook"));
        assertEquals("DA", matchPattern("h*n", "huhovdjestvarnomozedocisvastan"));
        assertEquals("DA", matchPattern("h*n", "honijezakon"));
        assertEquals("NE", matchPattern("h*n", "atila"));
        assertEquals("NE", matchPattern("h*n", "je"));
        assertEquals("NE", matchPattern("h*n", "bio"));
        assertEquals("DA", matchPattern("h*n", "hun"));
        assertEquals("DA", matchPattern("ab*cde", "abcde"));
        assertEquals("DA", matchPattern("ab*cde", "abasdfxcvsadfcde"));
        assertEquals("NE", matchPattern("ab*cde", "abasdfxcvsadfcdee"));
        assertEquals("NE", matchPattern("ab*bd", "abd"));
    }

    private String matchPattern(String pattern, String string) {
        String[] patterns = pattern.split("\\*");
        if (canMatch(string, patterns)) {
            return "DA";
        }
        return "NE";
    }

    private boolean canMatch(String string, String[] patterns) {
        if (string.length() < patterns[0].length() + patterns[1].length()) {
            return false;
        }
        return string.startsWith(patterns[0]) && string.endsWith(patterns[1]);
    }
}
~~~
