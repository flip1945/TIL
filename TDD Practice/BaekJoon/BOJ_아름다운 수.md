# 아름다운 수 (Bronze 2)

### 문제 설명

윤정이는 뭐든지 아름다운 것이 좋다고 생각한다. 그래서 윤정이는 사물을 볼 때 자신이 정한 방법으로 아름다운 정도를 평가한다. 윤정이는 수를 볼 때도 이런 아름다운 수의 정도를 따지는데, 윤정이에게 있어서 아름다운 수의 정도는 그 수를 이루고 있는 10진수의 서로 다른 숫자의 개수를 의미한다.  예를 들어 122이라는 수는 1과 2 라는 2개의 숫자로 이루어져 있으므로 아름다운 정도가 2가 된다. 윤정이의 방법으로 여러 수들의 아름다운 정도를 알아보자.

출처 : https://www.acmicpc.net/problem/2774

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            System.out.println(BeautifulNumber.from(scanner.nextLine()).get());
        }
    }
}

class BeautifulNumber {

    public static final String DELIMITER = "";
    private final int beautifulNumber;

    private BeautifulNumber(String number) {
        this.beautifulNumber = initBeautifulNumber(number);
    }

    private int initBeautifulNumber(String number) {
        return (int) Arrays.stream(number.split(DELIMITER))
                .distinct()
                .count();
    }

    public static BeautifulNumber from(String number) {
        return new BeautifulNumber(number);
    }

    public int get() {
        return this.beautifulNumber;
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

import java.util.*;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("주어진 숫자에서 서로 다른 10진수 숫자의 개수를 반환한다.")
    void getBeautifulNumberTest(String number, int expected) {
        BeautifulNumber sut = BeautifulNumber.from(number);

        int actual = sut.get();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of("1", 1),
                Arguments.of("12", 2),
                Arguments.of("123", 3),
                Arguments.of("11", 1),
                Arguments.of("101", 2),
                Arguments.of("1001", 2),
                Arguments.of("1234567890", 10)
        );
    }
}

class BeautifulNumber {

    public static final String DELIMITER = "";
    private final int beautifulNumber;

    private BeautifulNumber(String number) {
        this.beautifulNumber = initBeautifulNumber(number);
    }

    private int initBeautifulNumber(String number) {
        return (int) Arrays.stream(number.split(DELIMITER))
                .distinct()
                .count();
    }

    public static BeautifulNumber from(String number) {
        return new BeautifulNumber(number);
    }

    public int get() {
        return this.beautifulNumber;
    }
}
~~~
