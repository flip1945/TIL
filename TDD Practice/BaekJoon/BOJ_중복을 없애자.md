# 중복을 없애자 (Bronze 2)

### 문제 설명

Al의 초콜릿 망고 회사는 방문자들이 2d 단지에 얼마나 많은 초콜릿 망고가 있는지 추측할 수 있는 웹 사이트를 갖고 있다. 방문자들은 1부터 99까지의 수를 추측한 후 "제출" 버튼을 누르는데, 안타깝게도 서버로부터 응답시간이 종종 길어져 방문자들이 이성을 잃은 나머지 "제출"을 연타하는 사태가 발생한다. 이게 우리가 해결해야 할 문제다.

ACM의 직원을 도와 연타된 중복을 걸러보자.

출처 : https://www.acmicpc.net/problem/4592

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<Integer> s = inputSubmits(scanner, n);

        while (n != 0) {
            Submits submits = new Submits(s);
            System.out.println(submits.removeDuplicateSubmit());

            n = scanner.nextInt();
            s = inputSubmits(scanner, n);
        }
    }

    private static List<Integer> inputSubmits(Scanner scanner, int n) {
        List<Integer> submits = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            submits.add(scanner.nextInt());
        }
        return submits;
    }
}

class Submits {

    public static final String DELIMITER = " ";
    public static final String SUFFIX = " $";
    private final List<Integer> submits;

    public Submits(List<Integer> submits) {
        this.submits = new ArrayList<>(submits);
    }

    public String removeDuplicateSubmit() {
        return String.join(DELIMITER, removeDuplicate()) + SUFFIX;
    }

    private List<String> removeDuplicate() {
        List<String> result = new ArrayList<>();
        int prev = 0;
        for (int submit : submits) {
            if (submit != prev) {
                result.add(String.valueOf(submit));
            }
            prev = submit;
        }
        return result;
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
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSubmits")
    @DisplayName("연속된 중복을 제거한 원래의 제출 상태를 출력한다.")
    void numberGameTest(List<Integer> submits, String expected) {
        Submits sut = new Submits(submits);

        String actual = sut.removeDuplicateSubmit();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSubmits() {
        return Stream.of(
                Arguments.of(List.of(1, 2), "1 2 $"),
                Arguments.of(List.of(1, 3, 2), "1 3 2 $"),
                Arguments.of(List.of(1, 3, 2, 2), "1 3 2 $"),
                Arguments.of(List.of(2, 1, 3, 2, 2), "2 1 3 2 $"),
                Arguments.of(List.of(1, 22, 22, 22, 3), "1 22 3 $"),
                Arguments.of(List.of(98, 76, 20, 76), "98 76 20 76 $"),
                Arguments.of(List.of(19, 19, 35, 86, 86, 86), "19 35 86 $"),
                Arguments.of(List.of(7), "7 $")
        );
    }
}

class Submits {

    public static final String DELIMITER = " ";
    public static final String SUFFIX = " $";
    private final List<Integer> submits;

    public Submits(List<Integer> submits) {
        this.submits = new ArrayList<>(submits);
    }

    public String removeDuplicateSubmit() {
        return String.join(DELIMITER, removeDuplicate()) + SUFFIX;
    }

    private List<String> removeDuplicate() {
        List<String> result = new ArrayList<>();
        int prev = 0;
        for (int submit : submits) {
            if (submit != prev) {
                result.add(String.valueOf(submit));
            }
            prev = submit;
        }
        return result;
    }
}
~~~
