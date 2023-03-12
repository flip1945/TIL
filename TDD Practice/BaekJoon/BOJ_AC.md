# AC (Gold 5)

### 문제 설명

선영이는 주말에 할 일이 없어서 새로운 언어 AC를 만들었다. AC는 정수 배열에 연산을 하기 위해 만든 언어이다. 이 언어에는 두 가지 함수 R(뒤집기)과 D(버리기)가 있다.

함수 R은 배열에 있는 수의 순서를 뒤집는 함수이고, D는 첫 번째 수를 버리는 함수이다. 배열이 비어있는데 D를 사용한 경우에는 에러가 발생한다.

함수는 조합해서 한 번에 사용할 수 있다. 예를 들어, "AB"는 A를 수행한 다음에 바로 이어서 B를 수행하는 함수이다. 예를 들어, "RDD"는 배열을 뒤집은 다음 처음 두 수를 버리는 함수이다.

배열의 초기값과 수행할 함수가 주어졌을 때, 최종 결과를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/5430

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        int t = Integer.parseInt(bf.readLine());
        StringBuilder answer = new StringBuilder();

        for (int i = 0; i < t; i++) {
            String cmd = deleteCoupleReverse(bf.readLine());
            int countOfArray = Integer.parseInt(bf.readLine());
            List<String> numbers = parseArray(bf.readLine());

            if (isDeleteCommandCountOverArraySize(cmd, countOfArray)) {
                answer.append("error").append("\n");
                continue;
            }

            boolean isReverse = false;
            for (String c : cmd.split("")) {
                if (c.equals("R")) {
                    isReverse = !isReverse;
                } else if (c.equals("D")) {
                    pop(numbers, isReverse);
                }
            }

            if (isReverse) {
                Collections.reverse(numbers);
            }
            answer.append("[").append(String.join(",", numbers)).append("]").append("\n");
        }
        System.out.println(answer);
    }

    private static String deleteCoupleReverse(String cmd) {
        return cmd.replaceAll("RR", "");
    }

    private static List<String> parseArray(String array) {
        return new LinkedList<>(Arrays.asList(getBracketRemovedString(array).split(",")));
    }

    private static String getBracketRemovedString(String array) {
        return array.substring(1, array.length() - 1);
    }

    private static boolean isDeleteCommandCountOverArraySize(String cmd, int countOfArray) {
        return cmd.chars().filter(ch -> ch == 'D').count() > countOfArray;
    }

    private static void pop(List<String> numbers, boolean isReversed) {
        if (isReversed) {
            popRight(numbers);
            return;
        }
        popLeft(numbers);
    }

    private static void popLeft(List<String> numbers) {
        numbers.remove(0);
    }

    private static void popRight(List<String> numbers) {
        numbers.remove(numbers.size() - 1);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Arrays;
import java.util.Collections;
import java.util.LinkedList;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void deleteCoupleReverseTest() {
        String cmd1 = "RRD";
        String cmd2 = "RR";
        String cmd3 = "R";
        String cmd4 = "RDDR";
        String cmd5 = "DRRD";
        String cmd6 = "DRRDRR";
        String cmd7 = "DDRDRDR";

        assertEquals("D", deleteCoupleReverse(cmd1));
        assertEquals("", deleteCoupleReverse(cmd2));
        assertEquals("R", deleteCoupleReverse(cmd3));
        assertEquals("RDDR", deleteCoupleReverse(cmd4));
        assertEquals("DD", deleteCoupleReverse(cmd5));
        assertEquals("DD", deleteCoupleReverse(cmd6));
        assertEquals("DDRDRDR", deleteCoupleReverse(cmd7));
    }

    @Test
    void parseArrayTest() {
        String array1 = "[1,2,3,4]";
        String array2 = "[42]";
        String array3 = "[1,1,2,3,5,8]";

        assertEquals(List.of("1", "2", "3", "4"), parseArray(array1));
        assertEquals(List.of("42"), parseArray(array2));
        assertEquals(List.of("1", "1", "2", "3", "5", "8"), parseArray(array3));
    }

    @Test
    void isDeleteCommandCountOverArraySizeTest() {
        String cmd1 = "D";
        String cmd2 = "RDD";
        String cmd3 = "DD";
        int countOfArray1 = 0;
        int countOfArray2 = 3;
        int countOfArray3 = 1;

        assertEquals(true, isDeleteCommandCountOverArraySize(cmd1, countOfArray1));
        assertEquals(false, isDeleteCommandCountOverArraySize(cmd2, countOfArray2));
        assertEquals(true, isDeleteCommandCountOverArraySize(cmd3, countOfArray3));
    }

    @Test
    void popLeftTest() {
        List<String> numbers = parseArray("[1,2,3,4]");

        popLeft(numbers);
        assertEquals(List.of("2", "3", "4"), numbers);

        popLeft(numbers);
        assertEquals(List.of("3", "4"), numbers);

        popLeft(numbers);
        assertEquals(List.of("4"), numbers);

        popLeft(numbers);
        assertEquals(List.of(), numbers);
    }

    private void popLeft(List<String> numbers) {
        numbers.remove(0);
    }

    private boolean isDeleteCommandCountOverArraySize(String cmd, int countOfArray) {
        return cmd.chars().filter(ch -> ch == 'D').count() > countOfArray;
    }

    private List<String> parseArray(String array) {
        return new LinkedList<>(Arrays.asList(getBracketRemovedString(array).split(",")));
    }

    private String getBracketRemovedString(String array) {
        return array.substring(1, array.length() - 1);
    }

    private String deleteCoupleReverse(String cmd) {
        return cmd.replaceAll("RR", "");
    }
}
~~~
