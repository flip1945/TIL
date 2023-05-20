# 핸드폰 번호 궁합 (Bronze 1)

### 문제 설명

어린시절 다들 한 번씩은 이름으로 궁합을 본 적이 있을 것이다. 이것과 비슷한 방식으로 중앙대학교에는 핸드폰 번호 궁합을 보는 것이 유행이라고 한다.

핸드폰 번호 궁합을 보기 위해서는 먼저 궁합을 보고싶은 두 중앙대생 A와 B의 핸드폰 번호에서 맨 앞의 010과 "-"(하이픈)을 모두 제외한 후, A부터 시작하여 한 숫자씩 번갈아가면서 적는다. 그리고 인접한 두 숫자끼리 더한 값의 일의 자리를 두 숫자의 아래에 적어나가면서 마지막에 남는 숫자 2개로 궁합률을 구하게 된다.

예를 들어, 아래의 그림과 같이 A의 번호가 010-7475-9336 이고, B의 번호가 010-3619-5974 이면, 7346715995393764에서 시작하여 070386484822030, 77314022204233, 4045424424656, 449966866011, 83852442612, 1137686873, 240344450, 64378895, 0705674, 775131, 42644, 6808, 488, 26이 되어 둘은 26%의 궁합률을 가지게 된다.

<img src="https://upload.acmicpc.net/5769bcf0-cf82-46df-9dac-dcd0bb0dbeb0/-/crop/386x452/49,39/-/preview/">

위의 예시에서처럼 인접한 두 숫자를 더한 값이 두자리 정수가 되더라도, 일의 자리 숫자만 적는다. 가령 7과 3을 더하면 0을 적고, 4와 8을 더하면 2를 적는다.

중앙대학교에서 유행인 핸드폰 번호 궁합률을 알아보는 프로그램을 작성해보자. 단, A와 B의 핸드폰 번호는 다르다고 가정한다.

출처 : https://www.acmicpc.net/problem/17202

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String num1 = scanner.nextLine();
        String num2 = scanner.nextLine();
        Synergy synergy = Synergy.of(num1, num2);

        System.out.println(synergy.calculate().getSynergy());
    }
}

class Synergy {
    private final List<Integer> numbers;

    private Synergy(List<Integer> numbers) {
        this.numbers = numbers;
    }

    public static Synergy of(String number1, String number2) {
        return new Synergy(concatNumbers(number1, number2));
    }

    private static List<Integer> concatNumbers(String number1, String number2) {
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < number1.length(); i++) {
            result.add(Integer.parseInt(String.valueOf(number1.charAt(i))));
            result.add(Integer.parseInt(String.valueOf(number2.charAt(i))));
        }
        return result;
    }

    public Synergy calculate() {
        if (numbers.size() <= 2) {
            return new Synergy(numbers);
        }
        return nextSynergy().calculate();
    }

    private Synergy nextSynergy() {
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < numbers.size() - 1; i++) {
            result.add((numbers.get(i) + numbers.get(i + 1)) % 10);
        }
        return new Synergy(result);
    }

    public String getSynergy() {
        return numbers.get(0) + "" + numbers.get(1);
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
    @MethodSource("provideNumbers")
    @DisplayName("두 숫자의 궁합을 반환한다.")
    void prefixArraySortTest(String num1, String num2, String expected) {
        Synergy sut = Synergy.of(num1, num2);

        String actual = sut.calculate().getSynergy();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of("1", "2", "12"),
                Arguments.of("3", "4", "34"),
                Arguments.of("60", "88", "26"),
                Arguments.of("753", "711", "26"),
                Arguments.of("74759336", "36195974", "26"),
                Arguments.of("01234567", "12345678", "02")
        );
    }
}

class Synergy {
    private final List<Integer> numbers;

    private Synergy(List<Integer> numbers) {
        this.numbers = numbers;
    }

    public static Synergy of(String number1, String number2) {
        return new Synergy(concatNumbers(number1, number2));
    }

    private static List<Integer> concatNumbers(String number1, String number2) {
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < number1.length(); i++) {
            result.add(Integer.parseInt(String.valueOf(number1.charAt(i))));
            result.add(Integer.parseInt(String.valueOf(number2.charAt(i))));
        }
        return result;
    }

    public Synergy calculate() {
        if (numbers.size() <= 2) {
            return new Synergy(numbers);
        }
        return nextSynergy().calculate();
    }

    private Synergy nextSynergy() {
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < numbers.size() - 1; i++) {
            result.add((numbers.get(i) + numbers.get(i + 1)) % 10);
        }
        return new Synergy(result);
    }

    public String getSynergy() {
        return numbers.get(0) + "" + numbers.get(1);
    }
}
~~~
