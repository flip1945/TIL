# 방 번호 (Silver 5)

### 문제 설명

다솜이는 은진이의 옆집에 새로 이사왔다. 다솜이는 자기 방 번호를 예쁜 플라스틱 숫자로 문에 붙이려고 한다.

다솜이의 옆집에서는 플라스틱 숫자를 한 세트로 판다. 한 세트에는 0번부터 9번까지 숫자가 하나씩 들어있다. 다솜이의 방 번호가 주어졌을 때, 필요한 세트의 개수의 최솟값을 출력하시오. (6은 9를 뒤집어서 이용할 수 있고, 9는 6을 뒤집어서 이용할 수 있다.)

출처 : https://www.acmicpc.net/problem/1475

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        NumbersSet numbersSet = new NumbersSet(scanner.nextLine());
        System.out.println(numbersSet.getMaxNumber());
    }
}

class NumbersSet {
    public static final String COMPATIBLE_NUMBER_KEY = "69";
    public static final String COMPATIBLE_NUMBER1 = "6";
    public static final String COMPATIBLE_NUMBER2 = "9";
    private final Map<String, Integer> numbersSet;

    public NumbersSet(String roomNumber) {
        this.numbersSet = initNumbersSet(roomNumber);
    }

    private Map<String, Integer> initNumbersSet(String roomNumber) {
        Map<String, Integer> numberCount = new HashMap<>();
        for (String number : roomNumber.split("")) {
            numberCount.put(getNumber(number), getIncreasedCount(numberCount, number));
        }
        return numberCount;
    }

    private String getNumber(String number) {
        if (isCompatible(number)) {
            return COMPATIBLE_NUMBER_KEY;
        }
        return number;
    }

    private boolean isCompatible(String number) {
        return COMPATIBLE_NUMBER1.equals(number) || COMPATIBLE_NUMBER2.equals(number);
    }

    private Integer getIncreasedCount(Map<String, Integer> numberCount, String number) {
        if (isCompatible(number)) {
            return numberCount.getOrDefault(COMPATIBLE_NUMBER_KEY, 0) + 1;
        }
        return numberCount.getOrDefault(number, 0) + 1;
    }

    public int getMaxNumber() {
        int result = 0;
        for (Map.Entry<String, Integer> entry : numbersSet.entrySet()) {
            result = Math.max(result, getCount(entry));
        }
        return result;
    }

    private int getCount(Map.Entry<String, Integer> entry) {
        if (entry.getKey().equals(COMPATIBLE_NUMBER_KEY)) {
            return (int) Math.round((double) entry.getValue() / 2);
        }
        return entry.getValue();
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

import java.util.HashMap;
import java.util.Map;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideRoomNumbers")
    @DisplayName("방 번호를 만들기 위해 필요한 숫자 세트의 개수를 반환한다.")
    void getCountOfNumberSetOfRoomNumberTest(String roomNumber, int expected) {
        NumbersSet sut = new NumbersSet(roomNumber);

        int actual = sut.getMaxNumber();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideRoomNumbers() {
        return Stream.of(
                Arguments.of("1", 1),
                Arguments.of("11", 2),
                Arguments.of("12", 1),
                Arguments.of("123", 1),
                Arguments.of("9999", 2),
                Arguments.of("12635", 1),
                Arguments.of("888888", 6),
                Arguments.of("122", 2),
                Arguments.of("66999", 3)
        );
    }
}

class NumbersSet {
    public static final String COMPATIBLE_NUMBER_KEY = "69";
    public static final String COMPATIBLE_NUMBER1 = "6";
    public static final String COMPATIBLE_NUMBER2 = "9";
    private final Map<String, Integer> numbersSet;

    public NumbersSet(String roomNumber) {
        this.numbersSet = initNumbersSet(roomNumber);
    }

    private Map<String, Integer> initNumbersSet(String roomNumber) {
        Map<String, Integer> numberCount = new HashMap<>();
        for (String number : roomNumber.split("")) {
            numberCount.put(getNumber(number), getIncreasedCount(numberCount, number));
        }
        return numberCount;
    }

    private String getNumber(String number) {
        if (isCompatible(number)) {
            return COMPATIBLE_NUMBER_KEY;
        }
        return number;
    }

    private boolean isCompatible(String number) {
        return COMPATIBLE_NUMBER1.equals(number) || COMPATIBLE_NUMBER2.equals(number);
    }

    private Integer getIncreasedCount(Map<String, Integer> numberCount, String number) {
        if (isCompatible(number)) {
            return numberCount.getOrDefault(COMPATIBLE_NUMBER_KEY, 0) + 1;
        }
        return numberCount.getOrDefault(number, 0) + 1;
    }

    public int getMaxNumber() {
        int result = 0;
        for (Map.Entry<String, Integer> entry : numbersSet.entrySet()) {
            result = Math.max(result, getCount(entry));
        }
        return result;
    }

    private int getCount(Map.Entry<String, Integer> entry) {
        if (entry.getKey().equals(COMPATIBLE_NUMBER_KEY)) {
            return (int) Math.round((double) entry.getValue() / 2);
        }
        return entry.getValue();
    }
}
~~~
