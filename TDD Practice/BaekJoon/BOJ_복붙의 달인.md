# 복붙의 달인 (Silver 5)

### 문제 설명

한신이는 대학교에서 "복붙의 달인"으로 유명하다. 한신이는 타이핑 속도가 느리기 때문에 대학에서 가능한 모든 일을 복붙으로 해결한다. 그는 n개의 문자를 입력하는데 있어서 n초의 시간이 걸리지만 뛰어난 "붙여넣기" 스킬을 이용하면 어떠한 개수의 문자도 단 1초만에 타이핑 할 수 있다. 만약 한신이가 "bana"를 복사한 상태에서 "banana"를 타이핑한다면, "bana" 붙여넣기 1초, 'n' 입력, 'a' 입력으로 총 3초가 걸린다. 한신이가 클립보드에 저장한 p를 알고 있을 때 s를 입력하는데 걸리는 최소 시간을 계산해보자!

출처 : https://www.acmicpc.net/problem/11008

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            StringTokenizer st = new StringTokenizer(scanner.nextLine());
            Paste paste = new Paste(st.nextToken(), st.nextToken());
            System.out.println(paste.getMinPasteTime());
        }
    }
}

class Paste {

    private final String word;
    private final String pasteWord;

    public Paste(String word, String pasteWord) {
        this.word = word;
        this.pasteWord = pasteWord;
    }

    public int getMinPasteTime() {
        return getCountOfPaste() + getReducedString().length();
    }

    private int getCountOfPaste() {
        return (word.length() - getReducedString().length()) / pasteWord.length();
    }

    private String getReducedString() {
        return word.replaceAll(pasteWord, "");
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
    @MethodSource("provideStrings")
    @DisplayName("붙여넣기를 이용해 타이핑에 걸리는 최소시간을 반환한다.")
    void pasteTest(String word, String paste, int expected) {
        Paste sut = new Paste(word, paste);

        int actual = sut.getMinPasteTime();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideStrings() {
        return Stream.of(
                Arguments.of("a", "a", 1),
                Arguments.of("ab", "a", 2),
                Arguments.of("ab", "ab", 1),
                Arguments.of("banana", "bana", 3),
                Arguments.of("asakusa", "sa", 5),
                Arguments.of("asakusa", "abc", 7)
        );
    }
}

class Paste {

    private final String word;
    private final String pasteWord;

    public Paste(String word, String pasteWord) {
        this.word = word;
        this.pasteWord = pasteWord;
    }

    public int getMinPasteTime() {
        return getCountOfPaste() + getReducedString().length();
    }

    private int getCountOfPaste() {
        return (word.length() - getReducedString().length()) / pasteWord.length();
    }

    private String getReducedString() {
        return word.replaceAll(pasteWord, "");
    }
}
~~~
