# Gnome Sequencing (Bronze 4)

### 문제 설명

In the book All Creatures of Mythology, gnomes are kind, bearded creatures, while goblins tend to be bossy and simple-minded. The goblins like to harass the gnomes by making them line up in groups of three, ordered by the length of their beards. The gnomes, being of different physical heights, vary their arrangements to confuse the goblins. Therefore, the goblins must actually measure the beards in centimeters to see if everyone is lined up in order.

Your task is to write a program to assist the goblins in determining whether or not the gnomes are lined up properly, either from shortest to longest beard or from longest to shortest.

출처 : https://www.acmicpc.net/problem/4589

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Gnomes:");

        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            List<Integer> gnomes = List.of(scanner.nextInt(), scanner.nextInt(), scanner.nextInt());
            GnomeSequencing gnomeSequencing = new GnomeSequencing(gnomes);
            System.out.println(gnomeSequencing.isOrdered());
        }
    }
}

class GnomeSequencing {

    private static final String ORDERED = "Ordered";
    private static final String UNORDERED = "Unordered";

    private final List<Integer> gnomes;

    public GnomeSequencing(List<Integer> gnomes) {
        this.gnomes = gnomes;
    }

    public String isOrdered() {
        List<Integer> orderedGnomes = getSortedGnomes();
        List<Integer> reversedOrderedGnomes = getReverseSortedGnomes();
        return isOrdered(orderedGnomes, reversedOrderedGnomes) ? ORDERED : UNORDERED;
    }

    private boolean isOrdered(List<Integer> orderedGnomes, List<Integer> reversedGnomes) {
        return orderedGnomes.equals(gnomes) || reversedGnomes.equals(gnomes);
    }

    private List<Integer> getSortedGnomes() {
        return gnomes.stream()
                .sorted()
                .collect(Collectors.toList());
    }

    private List<Integer> getReverseSortedGnomes() {
        return gnomes.stream()
                .sorted(Comparator.reverseOrder())
                .collect(Collectors.toList());
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

import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideGnomes")
    @DisplayName("유전자 순서는 유전자의 순서를 확인한다.")
    void gnomeSequencingCheckGnomeOrder(List<Integer> gnomes, String expected) {
        GnomeSequencing sut = new GnomeSequencing(gnomes);

        String actual = sut.isOrdered();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideGnomes() {
        return Stream.of(
                Arguments.of(List.of(1, 2, 3), "Ordered"),
                Arguments.of(List.of(1, 3, 2), "Unordered"),
                Arguments.of(List.of(3, 2, 1), "Ordered"),
                Arguments.of(List.of(40, 62, 77), "Ordered"),
                Arguments.of(List.of(88, 62, 77), "Unordered"),
                Arguments.of(List.of(91, 33, 18), "Ordered")
        );
    }
}

class GnomeSequencing {

    private static final String ORDERED = "Ordered";
    private static final String UNORDERED = "Unordered";

    private final List<Integer> gnomes;

    public GnomeSequencing(List<Integer> gnomes) {
        this.gnomes = gnomes;
    }

    public String isOrdered() {
        List<Integer> orderedGnomes = getSortedGnomes();
        List<Integer> reversedOrderedGnomes = getReverseSortedGnomes();
        return isOrdered(orderedGnomes, reversedOrderedGnomes) ? ORDERED : UNORDERED;
    }

    private boolean isOrdered(List<Integer> orderedGnomes, List<Integer> reversedGnomes) {
        return orderedGnomes.equals(gnomes) || reversedGnomes.equals(gnomes);
    }

    private List<Integer> getSortedGnomes() {
        return gnomes.stream()
                .sorted()
                .collect(Collectors.toList());
    }

    private List<Integer> getReverseSortedGnomes() {
        return gnomes.stream()
                .sorted(Comparator.reverseOrder())
                .collect(Collectors.toList());
    }
}
~~~
