# 하얀 칸 (Bronze 2)

### 문제 설명

체스판은 8×8크기이고, 검정 칸과 하얀 칸이 번갈아가면서 색칠되어 있다. 가장 왼쪽 위칸 (0,0)은 하얀색이다. 체스판의 상태가 주어졌을 때, 하얀 칸 위에 말이 몇 개 있는지 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1100

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.Predicate;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<String> board = new ArrayList<>();
        for (int i = 0; i < 8; i++) {
            board.add(scanner.nextLine());
        }

        System.out.println(getCountOfChessPieceOnWhiteBoard(board));
    }
    
    static int getCountOfChessPieceOnWhiteBoard(List<String> board) {
        return IntStream.range(0, 8)
                .map(i -> getCountOfChessPieceOnWhiteBoardOfLine(board.get(i), i))
                .sum();
    }

    static int getCountOfChessPieceOnWhiteBoardOfLine(String boardLine, int i) {
        if (isEvenLine(i)) {
            return getCountOfLine(boardLine, Main::isEvenLine);
        }
        return getCountOfLine(boardLine, Main::isOddLine);
    }

    static boolean isEvenLine(int i) {
        return i % 2 == 0;
    }

    static boolean isOddLine(int i) {
        return i % 2 == 1;
    }

    static int getCountOfLine(String boardLine, Predicate<Integer> predicate) {
        return (int) IntStream.range(0, 8)
                .filter(i -> predicate.test(i) && isOnTheBoard(boardLine.charAt(i)))
                .count();
    }

    static boolean isOnTheBoard(char board) {
        return board == 'F';
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.*;
import java.util.function.Predicate;
import java.util.stream.IntStream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getCountOfChessPieceOnWhiteBoardTest() {
        assertEquals(1, getCountOfChessPieceOnWhiteBoard(List.of(".F.F...F", "F...F.F.", "...F.F.F", "F.F...F.", ".F...F..", "F...F.F.", ".F.F.F.F", "..FF..F.")));
        assertEquals(0, getCountOfChessPieceOnWhiteBoard(List.of("........", "........", "........", "........", "........", "........", "........", "........")));
        assertEquals(32, getCountOfChessPieceOnWhiteBoard(List.of("FFFFFFFF", "FFFFFFFF", "FFFFFFFF", "FFFFFFFF", "FFFFFFFF", "FFFFFFFF", "FFFFFFFF", "FFFFFFFF")));
        assertEquals(2, getCountOfChessPieceOnWhiteBoard(List.of("........", "..F.....", ".....F..", ".....F..", "........", "........", ".......F", ".F......")));
    }

    private int getCountOfChessPieceOnWhiteBoard(List<String> board) {
        return IntStream.range(0, 8)
                .map(i -> getCountOfChessPieceOnWhiteBoardOfLine(board.get(i), i))
                .sum();
    }

    private int getCountOfChessPieceOnWhiteBoardOfLine(String boardLine, int i) {
        if (isEvenLine(i)) {
            return getCountOfLine(boardLine, this::isEvenLine);
        }
        return getCountOfLine(boardLine, this::isOddLine);
    }

    private boolean isEvenLine(int i) {
        return i % 2 == 0;
    }

    private boolean isOddLine(int i) {
        return i % 2 == 1;
    }

    private int getCountOfLine(String boardLine, Predicate<Integer> predicate) {
        return (int) IntStream.range(0, 8)
                .filter(i -> predicate.test(i) && isOnTheBoard(boardLine.charAt(i)))
                .count();
    }

    private boolean isOnTheBoard(char board) {
        return board == 'F';
    }
}
~~~
