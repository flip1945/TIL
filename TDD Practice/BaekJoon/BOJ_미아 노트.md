# 미아 노트 (Silver 5)

### 문제 설명

미아는 과일을 좋아하는 소녀이다. 그녀의 비밀 노트에는 과일에 대해 그녀가 수집한 정보들이 가득하다.

평소와 다를 바 없이 과일들을 잔뜩 관찰하고 기쁜 마음으로 하교하던 어느 날, 친구가 뒤에서 덮치는 바람에 실수로 비밀 노트를 물에 빠뜨리고 말았다.

다행히 노트는 건질 수 있었지만, 노트에 적어두었던 정보들이 번지고 지워져버려 일부는 알아볼 수 없게 되었다.

노트에 적힌 문자열이 번진 패턴은 일정했는데, 가령 "abc" 문자가 세로로 3글자씩, 가로로 2글자씩 번진 경우는 다음과 같았다.

```
aabbcc
aabbcc
aabbcc
```

이 패턴을 이용해 문자열을 완전히 복원할 수 있을 것 같았지만, 아쉽게도 번진 문자열의 일부는 지워진 상태였다. 너무 많이 지워져버려서 해당 자리의 문자를 유추할 수 없는 경우, 완전히 문자열을 복원하지 못할 수도 있다.

미아는 자신이 아끼는 노트가 물에 빠진 바람에 매우 속상해하고 있다. 문자열을 최대한 완전히 복원해서 미아의 기를 살려주자!

출처 : https://www.acmicpc.net/problem/20114

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        int n = Integer.parseInt(st.nextToken());
        int h = Integer.parseInt(st.nextToken());
        int w = Integer.parseInt(st.nextToken());
        String[] strings = new String[h];
        for (int i = 0; i < h; i++) {
            strings[i] = scanner.nextLine();
        }

        System.out.println(getOriginNote(n, w, strings));
    }

    static String getOriginNote(int n, int w, String[] strings) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            sb.append(getOriginLetterOrBlurLetter(getSameBlurString(strings, i, w)));
        }
        return sb.toString();
    }

    static String getSameBlurString(String[] strings, int currentStringIndex, int sizeOfBlurLetter) {
        StringBuilder blurString = new StringBuilder();
        int startIndex = currentStringIndex * sizeOfBlurLetter;
        for (String string : strings) {
            blurString.append(string, startIndex, startIndex + sizeOfBlurLetter);
        }
        return blurString.toString();
    }

    static char getOriginLetterOrBlurLetter(String sameBlurString) {
        for (char letter : sameBlurString.toCharArray()) {
            if (letter != '?') {
                return letter;
            }
        }
        return '?';
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getOriginNoteTest() {
        assertEquals("abc", getOriginNote(3, 1, new String[]{"abc", "abc"}));
        assertEquals("?bc", getOriginNote(3, 1, new String[]{"?bc", "?bc"}));
        assertEquals("abc", getOriginNote(3, 1, new String[]{"abc", "?bc"}));
        assertEquals("a?c", getOriginNote(3, 1, new String[]{"a?c", "??c"}));
        assertEquals("???", getOriginNote(3, 1, new String[]{"???", "???"}));
        assertEquals("abc", getOriginNote(3, 2, new String[]{"a?????", "???bcc"}));
        assertEquals("fru?t?", getOriginNote(6, 3, new String[]{"???rrruuu???ttt???", "f?f?rruuu?????t???"}));
    }

    private String getOriginNote(int n, int w, String[] strings) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            sb.append(getOriginLetterOrBlurLetter(getSameBlurString(strings, i, w)));
        }
        return sb.toString();
    }

    private String getSameBlurString(String[] strings, int currentStringIndex, int sizeOfBlurLetter) {
        StringBuilder blurString = new StringBuilder();
        int startIndex = currentStringIndex * sizeOfBlurLetter;
        for (String string : strings) {
            blurString.append(string, startIndex, startIndex + sizeOfBlurLetter);
        }
        return blurString.toString();
    }

    private char getOriginLetterOrBlurLetter(String sameBlurString) {
        for (char letter : sameBlurString.toCharArray()) {
            if (letter != '?') {
                return letter;
            }
        }
        return '?';
    }
}
~~~
