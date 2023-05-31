# 반복수열 (Silver 4)

### 문제 설명

다음과 같이 정의된 수열이 있다.

* D\[1] = A
* D\[n] = D\[n-1]의 각 자리의 숫자를 P번 곱한 수들의 합
예를 들어 A=57, P=2일 때, 수열 D는 \[57, 74(=5^2+7^2=25+49), 65, 61, 37, 58, 89, 145, 42, 20, 4, 16, 37, …]이 된다. 그 뒤에는 앞서 나온 수들(57부터가 아니라 58부터)이 반복된다.

이와 같은 수열을 계속 구하다 보면 언젠가 이와 같은 반복수열이 된다. 이때, 반복되는 부분을 제외했을 때, 수열에 남게 되는 수들의 개수를 구하는 프로그램을 작성하시오. 위의 예에서는 \[57, 74, 65, 61]의 네 개의 수가 남게 된다.

출처 : https://www.acmicpc.net/problem/2331

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int a = scanner.nextInt();
        int p = scanner.nextInt();
        System.out.println(UniqueSequence.getUniqueNumbers(a, p));
    }
}

class UniqueSequence {
    public static int getUniqueNumbers(int startNumber, int power) {
        Set<Sequence> uniqueNumbers = new LinkedHashSet<>();
        Sequence sequence = new Sequence(startNumber, power);
        while (!uniqueNumbers.contains(sequence)) {
            uniqueNumbers.add(sequence);
            sequence = sequence.nextSequence();
        }
        return new ArrayList<>(uniqueNumbers).indexOf(sequence);
    }
}

class Sequence {
    private final int sequenceNumber;
    private final int power;

    public Sequence(int sequenceNumber, int power) {
        this.sequenceNumber = sequenceNumber;
        this.power = power;
    }

    public Sequence nextSequence() {
        return new Sequence(getNextSequence(), power);
    }

    private int getNextSequence() {
        int result = 0;
        int number = sequenceNumber;
        while (number > 0) {
            result += Math.pow((number % 10), power);
            number /= 10;
        }
        return result;
    }

    public int getSequenceNumber() {
        return sequenceNumber;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Sequence sequence = (Sequence) o;
        return sequenceNumber == sequence.sequenceNumber;
    }

    @Override
    public int hashCode() {
        return Objects.hash(sequenceNumber);
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
    @MethodSource("provideStartNumberAndPowers")
    @DisplayName("반복 수열 중 중복되지 않는 숫자의 개수를 반환한다.")
    void getUniqueNumbersTest(int startNumber, int power, int expected) {
        int actual = UniqueSequence.getUniqueNumbers(startNumber, power);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideStartNumberAndPowers() {
        return Stream.of(
                Arguments.of(1, 1, 0),
                Arguments.of(2, 2, 1),
                Arguments.of(3, 2, 5),
                Arguments.of(57, 2, 4)
        );
    }

    @ParameterizedTest
    @MethodSource("provideNumberAndPowers")
    @DisplayName("각 자리수를 지수만큽 곱하고 그 수를 모두 더한 합을 반환한다.")
    void getNextSequenceTest(int number, int power, int expected) {
        Sequence sut = new Sequence(number, power);

        int actual = sut.nextSequence().getSequenceNumber();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumberAndPowers() {
        return Stream.of(
                Arguments.of(1, 2, 1),
                Arguments.of(2, 2, 4),
                Arguments.of(16, 2, 37),
                Arguments.of(16, 3, 217),
                Arguments.of(65, 2, 61)
        );
    }
}

class UniqueSequence {
    public static int getUniqueNumbers(int startNumber, int power) {
        Set<Sequence> uniqueNumbers = new LinkedHashSet<>();
        Sequence sequence = new Sequence(startNumber, power);
        while (!uniqueNumbers.contains(sequence)) {
            uniqueNumbers.add(sequence);
            sequence = sequence.nextSequence();
        }
        return new ArrayList<>(uniqueNumbers).indexOf(sequence);
    }
}

class Sequence {
    private final int sequenceNumber;
    private final int power;

    public Sequence(int sequenceNumber, int power) {
        this.sequenceNumber = sequenceNumber;
        this.power = power;
    }

    public Sequence nextSequence() {
        return new Sequence(getNextSequence(), power);
    }

    private int getNextSequence() {
        int result = 0;
        int number = sequenceNumber;
        while (number > 0) {
            result += Math.pow((number % 10), power);
            number /= 10;
        }
        return result;
    }

    public int getSequenceNumber() {
        return sequenceNumber;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Sequence sequence = (Sequence) o;
        return sequenceNumber == sequence.sequenceNumber;
    }

    @Override
    public int hashCode() {
        return Objects.hash(sequenceNumber);
    }
}
~~~
