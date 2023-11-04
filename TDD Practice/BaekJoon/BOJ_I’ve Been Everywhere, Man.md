# I’ve Been Everywhere, Man (Silver 5)

### 문제 설명

Alice travels a lot for her work. Each time she travels, she visits a single city before returning home.

Someone recently asked her “how many different cities have you visited for work?” Thankfully Alice has kept a log of her trips. Help Alice figure out the number of cities she has visited at least once.

출처 : https://www.acmicpc.net/problem/11645

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < t; i++) {
            int n = Integer.parseInt(scanner.nextLine());
            List<String> visitCities = inputCities(scanner, n);
            Travel travel = new Travel(visitCities);

            System.out.println(travel.visitCityCount());
        }
    }

    private static List<String> inputCities(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Travel {

    private final List<String> visitCities;

    public Travel(List<String> visitCities) {
        this.visitCities = visitCities;
    }

    public int visitCityCount() {
        return new HashSet<>(visitCities).size();
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

import java.util.HashSet;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideCities")
    @DisplayName("중복 없이 방문한 도시의 개수를 반환한다.")
    void travelReturnsDistinctVisitCityCount(List<String> cities, int expected) {
        Travel sut = new Travel(cities);

        int actual = sut.visitCityCount();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideCities() {
        return Stream.of(
                Arguments.of(List.of("seoul"), 1),
                Arguments.of(List.of("seoul", "busan"), 2),
                Arguments.of(List.of("seoul", "busan", "seoul"), 2),
                Arguments.of(List.of("edmonton", "edmonton", "edmonton"), 1),
                Arguments.of(List.of("saskatoon", "toronto", "winnipeg", "toronto", "vancouver", "saskatoon", "toronto"), 4)
        );
    }
}

class Travel {

    private final List<String> visitCities;

    public Travel(List<String> visitCities) {
        this.visitCities = visitCities;
    }

    public int visitCityCount() {
        return new HashSet<>(visitCities).size();
    }
}
~~~
