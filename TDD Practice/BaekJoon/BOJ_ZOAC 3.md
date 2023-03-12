# ZOAC 3 (Silver 4)

### 문제 설명

2020년 12월, 세 번째로 개최된 ZOAC의 오프닝을 맡은 성우는 누구보다 빠르게 ZOAC를 알리려 한다.

하지만 안타깝게도 성우는 독수리타법이다!

* 독수리 타법이란 양 손의 검지손가락만을 이용해 타자를 치는 타법이다.
* 성우는 한글 자음 쪽 자판은 왼손 검지손가락으로 입력하고, 한글 모음 쪽 자판은 오른손 검지손가락으로 입력한다.
* a의 좌표가 (x1, y1)이고, b의 좌표가 (x2, y2)일 때, a에 위치한 성우의 손가락이 b로 이동하는 데에는 a와 b의 택시 거리 |x1-x2|+|y1-y2| 만큼의 시간이 걸린다.
* 각 키를 누르는 데에는 1의 시간이 걸린다.
* 성우는 두 손을 동시에 움직일 수 없다.
* 성우가 사용하는 키보드는 쿼티식 키보드이며, 아래 그림처럼 생겼다.

<img src='https://upload.acmicpc.net/408ea292-3a7e-4b25-b5ec-d6a85f82a6ce/-/preview/'>

바쁜 성우를 위하여 해당 문자열을 출력하는 데 걸리는 시간의 최솟값을 구해보자.

출처 : https://www.acmicpc.net/problem/20436

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(bf.readLine());
        String left = st.nextToken();
        String right = st.nextToken();

        System.out.println(new KeyBoard(left, right, bf.readLine()).getTotalInputTime());
    }
}

class KeyBoard {
    private static final Map<String, Point> KEY_MAP = new HashMap<>() {{
        put("z", new Point(0, 0));
        put("x", new Point(1, 0));
        put("c", new Point(2, 0));
        put("v", new Point(3, 0));
        put("b", new Point(4, 0));
        put("n", new Point(5, 0));
        put("m", new Point(6, 0));
        put("a", new Point(0, 1));
        put("s", new Point(1, 1));
        put("d", new Point(2, 1));
        put("f", new Point(3, 1));
        put("g", new Point(4, 1));
        put("h", new Point(5, 1));
        put("j", new Point(6, 1));
        put("k", new Point(7, 1));
        put("l", new Point(8, 1));
        put("q", new Point(0, 2));
        put("w", new Point(1, 2));
        put("e", new Point(2, 2));
        put("r", new Point(3, 2));
        put("t", new Point(4, 2));
        put("y", new Point(5, 2));
        put("u", new Point(6, 2));
        put("i", new Point(7, 2));
        put("o", new Point(8, 2));
        put("p", new Point(9, 2));
    }};
    private static final Set<String> RIGHT_KEY_SET = Set.of("b", "n", "m", "h", "j", "k", "l", "y", "u", "i", "o", "p");
    private String leftHand;
    private String rightHand;
    private final String keyString;

    public KeyBoard(String leftHand, String rightHand, String keyString) {
        this.leftHand = leftHand;
        this.rightHand = rightHand;
        this.keyString = keyString;
    }

    public int getKeyDistance(String currentKey, String targetKey) {
        Point currentPoint = KEY_MAP.get(currentKey);
        Point targetPoint = KEY_MAP.get(targetKey);
        return Math.abs(currentPoint.x - targetPoint.x) + Math.abs(currentPoint.y - targetPoint.y);
    }

    public int getTotalInputTime() {
        int resultTime = 0;
        for (String key : keyString.split("")) {
            resultTime += getKeyInputTime(key);
        }
        return resultTime;
    }

    private int getKeyInputTime(String key) {
        int inputTime;
        if (RIGHT_KEY_SET.contains(key)) {
            inputTime = getKeyDistance(rightHand, key) + 1;
            rightHand = key;
            return inputTime;
        }
        inputTime = getKeyDistance(leftHand, key) + 1;
        leftHand = key;
        return inputTime;
    }
}

class Point {
    int x;
    int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
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
    void keyDistanceTest() {
        String currentKey = "z";
        String targetKey1 = "z";
        String targetKey2 = "x";
        String targetKey3 = "p";
        String targetKey4 = "o";

        KeyBoard keyBoard = new KeyBoard("z", "o", "");

        assertEquals(0, keyBoard.getKeyDistance(currentKey, targetKey1));
        assertEquals(1, keyBoard.getKeyDistance(currentKey, targetKey2));
        assertEquals(11, keyBoard.getKeyDistance(currentKey, targetKey3));
        assertEquals(10, keyBoard.getKeyDistance(currentKey, targetKey4));
    }

    @Test
    void keyBoardInputTimeTest() {
        String left1 = "z";
        String left2 = "a";
        String left3 = "s";
        String right1 = "o";
        String right2 = "h";

        assertEquals(8, new KeyBoard(left1, right1, "zoac").getTotalInputTime());
        assertEquals(2, new KeyBoard(left1, right1, "zo").getTotalInputTime());
        assertEquals(9, new KeyBoard(left1, right1, "zoca").getTotalInputTime());
        assertEquals(7, new KeyBoard(left1, right1, "gg").getTotalInputTime());
        assertEquals(8, new KeyBoard(left1, right1, "zgg").getTotalInputTime());
        assertEquals(7, new KeyBoard(left2, right2, "agg").getTotalInputTime());
        assertEquals(16, new KeyBoard(left3, right2, "adxwtt").getTotalInputTime());
    }
}

class KeyBoard {
    private static final Map<String, Point> KEY_MAP = new HashMap<>() {{
        put("z", new Point(0, 0));
        put("x", new Point(1, 0));
        put("c", new Point(2, 0));
        put("v", new Point(3, 0));
        put("b", new Point(4, 0));
        put("n", new Point(5, 0));
        put("m", new Point(6, 0));
        put("a", new Point(0, 1));
        put("s", new Point(1, 1));
        put("d", new Point(2, 1));
        put("f", new Point(3, 1));
        put("g", new Point(4, 1));
        put("h", new Point(5, 1));
        put("j", new Point(6, 1));
        put("k", new Point(7, 1));
        put("l", new Point(8, 1));
        put("q", new Point(0, 2));
        put("w", new Point(1, 2));
        put("e", new Point(2, 2));
        put("r", new Point(3, 2));
        put("t", new Point(4, 2));
        put("y", new Point(5, 2));
        put("u", new Point(6, 2));
        put("i", new Point(7, 2));
        put("o", new Point(8, 2));
        put("p", new Point(9, 2));
    }};
    private static final Set<String> RIGHT_KEY_SET = Set.of("b", "n", "m", "h", "j", "k", "l", "y", "u", "i", "o", "p");
    private String leftHand;
    private String rightHand;
    private final String keyString;

    public KeyBoard(String leftHand, String rightHand, String keyString) {
        this.leftHand = leftHand;
        this.rightHand = rightHand;
        this.keyString = keyString;
    }

    public int getKeyDistance(String currentKey, String targetKey) {
        Point currentPoint = KEY_MAP.get(currentKey);
        Point targetPoint = KEY_MAP.get(targetKey);
        return Math.abs(currentPoint.x - targetPoint.x) + Math.abs(currentPoint.y - targetPoint.y);
    }

    public int getTotalInputTime() {
        int resultTime = 0;
        for (String key : keyString.split("")) {
            resultTime += getKeyInputTime(key);
        }
        return resultTime;
    }

    private int getKeyInputTime(String key) {
        int inputTime;
        if (RIGHT_KEY_SET.contains(key)) {
            inputTime = getKeyDistance(rightHand, key) + 1;
            rightHand = key;
            return inputTime;
        }
        inputTime = getKeyDistance(leftHand, key) + 1;
        leftHand = key;
        return inputTime;
    }
}

class Point {
    int x;
    int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
~~~
