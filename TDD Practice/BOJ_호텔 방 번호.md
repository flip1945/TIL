# 호텔 방 번호 (Silver 5)

### 문제 설명

선영이는 집 호수에 반복되는 숫자가 있는 경우에는 그 집에 사는 사람에게 불운이 찾아온다고 믿는다. 따라서, 선영이는 838호나 1004호와 같이 한 숫자가 두 번 이상 들어있는 집에는 절대 살지 않을 것이다.

2050년, 선영이는 한국에서 가장 돈이 많은 사람이 되었다. 그녀는 해변가에 새로운 호텔을 하나 지으려고 한다. 하지만, 투숙객에게 불운이 찾아오는 것을 피하기 위해서 반복되는 숫자가 없게 방 번호를 만들려고 한다.

정부는 선영이의 호텔 방 번호는 N보다 크거나 같고, M보다 작거나 같아야 한다는 조건을 걸고 신축 허가를 내주었다. 선영이의 새 호텔에는 방이 최대 몇 개 있을 수 있을까? (두 방이 같은 방 번호를 사용할 수 없다)

출처 : https://www.acmicpc.net/problem/5671

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNext()) {
            int n = scanner.nextInt();
            int m = scanner.nextInt();
            System.out.println(solution(n, m));
        }
    }

    static int solution(int startRoomNumber, int endRoomNumber) {
        return (int) IntStream.rangeClosed(startRoomNumber, endRoomNumber)
                .filter(Main::canHotelRoomNumber)
                .count();
    }

    static boolean canHotelRoomNumber(int roomNumber) {
        Set<Character> roomNumberLetters = new HashSet<>();
        for (char number : String.valueOf(roomNumber).toCharArray()) {
            if (roomNumberLetters.contains(number)) {
                return false;
            }
            roomNumberLetters.add(number);
        }
        return true;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.HashSet;
import java.util.Set;
import java.util.stream.IntStream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void canHotelRoomNumberTest() {
        assertEquals(true, canHotelRoomNumber(21));
        assertEquals(false, canHotelRoomNumber(22));
        assertEquals(true, canHotelRoomNumber(23));
        assertEquals(true, canHotelRoomNumber(24));
        assertEquals(false, canHotelRoomNumber(121));
        assertEquals(false, canHotelRoomNumber(122));
        assertEquals(true, canHotelRoomNumber(123));
    }

    @Test
    void integrationTest() {
        assertEquals(14, solution(87, 104));
        assertEquals(0, solution(989, 1022));
        assertEquals(3, solution(22, 25));
        assertEquals(1, solution(1234, 1234));
    }

    public int solution(int startRoomNumber, int endRoomNumber) {
        return (int) IntStream.rangeClosed(startRoomNumber, endRoomNumber)
                .filter(this::canHotelRoomNumber)
                .count();
    }

    private boolean canHotelRoomNumber(int roomNumber) {
        Set<Character> roomNumberLetters = new HashSet<>();
        for (char number : String.valueOf(roomNumber).toCharArray()) {
            if (roomNumberLetters.contains(number)) {
                return false;
            }
            roomNumberLetters.add(number);
        }
        return true;
    }
}
~~~
