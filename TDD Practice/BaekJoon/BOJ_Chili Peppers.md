# Chili Peppers (Bronze 4)

### 문제 설명

Ron is cooking chili using an assortment of peppers.

The spiciness of a pepper is measured in Scoville Heat Units (SHU). Ron's chili is currently not spicy at all, but each time Ron adds a pepper, the total spiciness of the chili increases by the SHU value of that pepper.

The SHU values of the peppers available to Ron are shown in the following table:

|Pepper Name|Scolville Heat Units|
|-|-|
|Poblano|	1500|
|Mirasol|	6000|
|Serrano|	15500|
|Cayenne|	40000|
|Thai|	75000|
|Habanero|	125000|

Your job is to determine the total spiciness of Ron's chili after he has finished adding peppers.

출처 : https://www.acmicpc.net/problem/28249

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        Peppers peppers = new Peppers(inputPeppers(scanner, n));
        System.out.println(peppers.totalSpiciness());
    }

    private static List<String> inputPeppers(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Peppers {

    private final List<String> peppers;

    public Peppers(List<String> peppers) {
        this.peppers = peppers;
    }

    public int totalSpiciness() {
        return peppers.stream()
                .mapToInt(this::getScoville)
                .sum();
    }

    private int getScoville(String pepper) {
        return Pepper.valueOf(pepper.toUpperCase()).getScoville();
    }
}

enum Pepper {
    POBLANO(1500),
    MIRASOL(6000),
    SERRANO(15500),
    CAYENNE(40000),
    THAI(75000),
    HABANERO(125000);

    private final int scoville;

    Pepper(int scoville) {
        this.scoville = scoville;
    }

    public int getScoville() {
        return scoville;
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

import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("providePeppers")
    @DisplayName("고추들이 주어지면 매운맛의 합을 반환한다.")
    void peppersReturnsTotalSpicinessOfPeppers(List<String> peppers, int expected) {
        Peppers sut = new Peppers(peppers);

        int actual = sut.totalSpiciness();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> providePeppers() {
        return Stream.of(
                Arguments.of(List.of("Poblano"), 1500),
                Arguments.of(List.of("Poblano", "Poblano"), 3000),
                Arguments.of(List.of("Poblano", "Mirasol"), 7500),
                Arguments.of(List.of("Poblano", "Mirasol", "Serrano"), 23000),
                Arguments.of(List.of("Poblano", "Mirasol", "Serrano", "Cayenne", "Thai", "Habanero"), 263000)
        );
    }
}

class Peppers {

    private final List<String> peppers;

    public Peppers(List<String> peppers) {
        this.peppers = peppers;
    }

    public int totalSpiciness() {
        return peppers.stream()
                .mapToInt(this::getScoville)
                .sum();
    }

    private int getScoville(String pepper) {
        return Pepper.valueOf(pepper.toUpperCase()).getScoville();
    }
}

enum Pepper {
    POBLANO(1500),
    MIRASOL(6000),
    SERRANO(15500),
    CAYENNE(40000),
    THAI(75000),
    HABANERO(125000);

    private final int scoville;

    Pepper(int scoville) {
        this.scoville = scoville;
    }

    public int getScoville() {
        return scoville;
    }
}
~~~
