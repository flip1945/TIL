# 세로읽기 (Bronze 1)

### 문제 설명

아직 글을 모르는 영석이가 벽에 걸린 칠판에 자석이 붙어있는 글자들을 붙이는 장난감을 가지고 놀고 있다. 

이 장난감에 있는 글자들은 영어 대문자 ‘A’부터 ‘Z’, 영어 소문자 ‘a’부터 ‘z’, 숫자 ‘0’부터 ‘9’이다. 영석이는 칠판에 글자들을 수평으로 일렬로 붙여서 단어를 만든다. 다시 그 아래쪽에 글자들을 붙여서 또 다른 단어를 만든다. 이런 식으로 다섯 개의 단어를 만든다. 아래 그림 1은 영석이가 칠판에 붙여 만든 단어들의 예이다. 

~~~
A A B C D D
a f z z 
0 9 1 2 1
a 8 E W g 6
P 5 h 3 k x
~~~
<그림 1>

한 줄의 단어는 글자들을 빈칸 없이 연속으로 나열해서 최대 15개의 글자들로 이루어진다. 또한 만들어진 다섯 개의 단어들의 글자 개수는 서로 다를 수 있다. 

심심해진 영석이는 칠판에 만들어진 다섯 개의 단어를 세로로 읽으려 한다. 세로로 읽을 때, 각 단어의 첫 번째 글자들을 위에서 아래로 세로로 읽는다. 다음에 두 번째 글자들을 세로로 읽는다. 이런 식으로 왼쪽에서 오른쪽으로 한 자리씩 이동 하면서 동일한 자리의 글자들을 세로로 읽어 나간다. 위의 그림 1의 다섯 번째 자리를 보면 두 번째 줄의 다섯 번째 자리의 글자는 없다. 이런 경우처럼 세로로 읽을 때 해당 자리의 글자가 없으면, 읽지 않고 그 다음 글자를 계속 읽는다. 그림 1의 다섯 번째 자리를 세로로 읽으면 D1gk로 읽는다. 

그림 1에서 영석이가 세로로 읽은 순서대로 글자들을 공백 없이 출력하면 다음과 같다:

Aa0aPAf985Bz1EhCz2W3D1gkD6x

칠판에 붙여진 단어들이 주어질 때, 영석이가 세로로 읽은 순서대로 글자들을 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/10798

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<String> texts = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            texts.add(scanner.nextLine());
        }

        VerticalParser verticalParser = new VerticalParser(texts);
        System.out.println(verticalParser.parse());
    }
}

class VerticalParser {
    private final List<String> horizontalText;

    public VerticalParser(List<String> horizontalText) {
        this.horizontalText = horizontalText;
    }

    public String parse() {
        return convertVerticalTexts();
    }

    private String convertVerticalTexts() {
        StringBuilder verticalText = new StringBuilder();
        for (int i = 0; i < 15; i++) {
            convertVerticalText(verticalText, i);
        }
        return verticalText.toString();
    }

    private void convertVerticalText(StringBuilder verticalText, int i) {
        for (int j = 0; j < 5; j++) {
            if (horizontalText.get(j).length() > i) {
                verticalText.append(horizontalText.get(j).charAt(i));
            }
        }
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
    void textListTest() {
        String text1 = "ABCDE\nabcde\n01234\nFGHIJ\nfghij";
        String text2 = "AABCDD\nafzz\n09121\na8EWg6\nP5h3kx";

        List<String> result1 = splitText(text1);
        List<String> result2 = splitText(text2);

        assertEquals("ABCDE", result1.get(0));
        assertEquals("abcde", result1.get(1));
        assertEquals("01234", result1.get(2));
        assertEquals("FGHIJ", result1.get(3));
        assertEquals("fghij", result1.get(4));

        assertEquals("AABCDD", result2.get(0));
        assertEquals("afzz", result2.get(1));
        assertEquals("09121", result2.get(2));
        assertEquals("a8EWg6", result2.get(3));
        assertEquals("P5h3kx", result2.get(4));
    }

    @Test
    void verticalParseTest() {
        List<String> horizontalText1 = splitText("ABCDE\nabcde\n01234\nFGHIJ\nfghij");
        List<String> horizontalText2 = splitText("AABCDD\nafzz\n09121\na8EWg6\nP5h3kx");
        List<String> horizontalText3 = splitText("a\ns\nd\nf\ng");
        VerticalParser verticalParser1 = new VerticalParser(horizontalText1);
        VerticalParser verticalParser2 = new VerticalParser(horizontalText2);
        VerticalParser verticalParser3 = new VerticalParser(horizontalText3);

        assertEquals("Aa0FfBb1GgCc2HhDd3IiEe4Jj", verticalParser1.parse());
        assertEquals("Aa0aPAf985Bz1EhCz2W3D1gkD6x", verticalParser2.parse());
        assertEquals("asdfg", verticalParser3.parse());
    }

    private List<String> splitText(String text) {
        return Arrays.asList(text.split("\n"));
    }
}

class VerticalParser {
    private final List<String> horizontalText;

    public VerticalParser(List<String> horizontalText) {
        this.horizontalText = horizontalText;
    }

    public String parse() {
        return convertVerticalTexts();
    }

    private String convertVerticalTexts() {
        StringBuilder verticalText = new StringBuilder();
        for (int i = 0; i < 15; i++) {
            convertVerticalText(verticalText, i);
        }
        return verticalText.toString();
    }

    private void convertVerticalText(StringBuilder verticalText, int i) {
        for (int j = 0; j < 5; j++) {
            if (horizontalText.get(j).length() > i) {
                verticalText.append(horizontalText.get(j).charAt(i));
            }
        }
    }
}
~~~
