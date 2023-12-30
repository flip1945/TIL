# 진주로 가자! (Easy) (Bronze 3)

### 문제 설명

**이 문제는 "진주로 가자! (Hard)" 문제와 입력으로 주어지는 수의 범위를 제외하면 같은 문제이다.**

서울살이에 지쳐버린 경상국립대 졸업생 보선이는 대학생이었던 시절이 그리워졌고, 오랜만에 경상국립대가 있는 진주에 가고 싶어졌다. 그래서 보선이는 진주로 당일치기 나들이를 가기 위해 무작정 서울 터미널에 도착했다.

서울 터미널에는 
$N$개의 교통편이 있다. 각 교통편의 정보는 도착지와 요금으로 이루어져 있으며, 모든 도착지는 서로 다르다. 그리고 주어지는 도착지에는 진주로 가는 교통편을 의미하는 jinju가 반드시 존재한다.

요즘 물가 인상이 걱정되는 보선이는 진주로 가는 교통편의 요금을 알아보면서 그보다 비싼 교통편의 개수 또한 같이 알아보려고 한다. 하지만 보선이는 이미 지쳐버린 상태라 
$N$개의 교통편을 살펴볼 힘이 없었다. 그래서 보선이는 자신이 알아보고자 한 정보를 우리에게 대신 알아봐달라고 부탁했다.

자, 이제 보선이의 부탁을 들어주자.

출처 : https://www.acmicpc.net/problem/31009

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

        PublicTransportation publicTransportation = PublicTransportation.from(inputTransportations(scanner, n));
        System.out.println(publicTransportation.priceOfJinju());
        System.out.println(publicTransportation.countOfMoreExpansiveTransportationThanJinju());
    }

    private static List<String> inputTransportations(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class PublicTransportation {
    private static final String JINJU = "jinju";

    private final List<Transportation> transportations;

    public PublicTransportation(List<Transportation> transportations) {
        this.transportations = transportations;
    }

    public static PublicTransportation from(List<String> transportations) {
        return new PublicTransportation(initTransportations(transportations));
    }

    private static List<Transportation> initTransportations(List<String> transportations) {
        return transportations.stream()
                .map(Transportation::from)
                .collect(Collectors.toList());
    }

    public int countOfMoreExpansiveTransportationThanJinju() {
        return (int) transportations.stream()
                .map(Transportation::getPrice)
                .filter(this::isMoreExpansiveThanJinju)
                .count();
    }

    private boolean isMoreExpansiveThanJinju(Integer price) {
        return price > priceOfJinju();
    }

    public int priceOfJinju() {
        return transportations.stream()
                .filter(this::isDestinationJinju)
                .findFirst()
                .map(Transportation::getPrice)
                .orElseThrow();
    }

    private boolean isDestinationJinju(Transportation transportation) {
        return transportation.getDestination().equals(JINJU);
    }
}

class Transportation {
    private static final String DELIMITER = " ";
    private static final int DESTINATION_INDEX = 0;
    private static final int PRICE_INDEX = 1;

    private final String destination;
    private final int price;

    public Transportation(String destination, int price) {
        this.destination = destination;
        this.price = price;
    }

    public static Transportation from(String transportation) {
        String[] split = transportation.split(DELIMITER);
        return new Transportation(split[DESTINATION_INDEX], Integer.parseInt(split[PRICE_INDEX]));
    }

    public String getDestination() {
        return destination;
    }

    public int getPrice() {
        return price;
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
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideTransportationsAndJinjuPrice")
    @DisplayName("교통편이 주어지면 진주로 가는 교통편의 요금을 반환한다.")
    void publicTransportationReturnsPriceOfJinju(List<String> transportationsAndPrices, int expected) {
        PublicTransportation sut = PublicTransportation.from(transportationsAndPrices);

        int actual = sut.priceOfJinju();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideTransportationsAndJinjuPrice() {
        return Stream.of(
                Arguments.of(List.of("jinju 10"), 10),
                Arguments.of(List.of("jinju 100"), 100),
                Arguments.of(List.of("suwon 5", "jinju 123"), 123),
                Arguments.of(List.of("changwon 100", "incheon 70", "jinju 90", "haenam 530", "gangneung 660"), 90)
        );
    }

    @ParameterizedTest
    @MethodSource("provideTransportationsAndCountOfMoreExpansiveTransportationThanJinju")
    @DisplayName("교통편이 주어지면 진주로 가는 교통편보다 비싼 요금의 개수를 반환한다.")
    void publicTransportationReturnsCountOfMoreExpansiveTransportationThanJinju(List<String> transportationsAndPrices, int expected) {
        PublicTransportation sut = PublicTransportation.from(transportationsAndPrices);

        int actual = sut.countOfMoreExpansiveTransportationThanJinju();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideTransportationsAndCountOfMoreExpansiveTransportationThanJinju() {
        return Stream.of(
                Arguments.of(List.of("jinju 10"), 0),
                Arguments.of(List.of("suwon 5", "jinju 123"), 0),
                Arguments.of(List.of("suwon 5", "jinju 1"), 1),
                Arguments.of(List.of("changwon 100", "incheon 70", "jinju 90", "haenam 530", "gangneung 660"), 3)
        );
    }
}

class PublicTransportation {
    private static final String JINJU = "jinju";

    private final List<Transportation> transportations;

    public PublicTransportation(List<Transportation> transportations) {
        this.transportations = transportations;
    }

    public static PublicTransportation from(List<String> transportations) {
        return new PublicTransportation(initTransportations(transportations));
    }

    private static List<Transportation> initTransportations(List<String> transportations) {
        return transportations.stream()
                .map(Transportation::from)
                .collect(Collectors.toList());
    }

    public int countOfMoreExpansiveTransportationThanJinju() {
        return (int) transportations.stream()
                .map(Transportation::getPrice)
                .filter(this::isMoreExpansiveThanJinju)
                .count();
    }

    private boolean isMoreExpansiveThanJinju(Integer price) {
        return price > priceOfJinju();
    }

    public int priceOfJinju() {
        return transportations.stream()
                .filter(this::isDestinationJinju)
                .findFirst()
                .map(Transportation::getPrice)
                .orElseThrow();
    }

    private boolean isDestinationJinju(Transportation transportation) {
        return transportation.getDestination().equals(JINJU);
    }
}

class Transportation {
    private static final String DELIMITER = " ";
    private static final int DESTINATION_INDEX = 0;
    private static final int PRICE_INDEX = 1;

    private final String destination;
    private final int price;

    public Transportation(String destination, int price) {
        this.destination = destination;
        this.price = price;
    }

    public static Transportation from(String transportation) {
        String[] split = transportation.split(DELIMITER);
        return new Transportation(split[DESTINATION_INDEX], Integer.parseInt(split[PRICE_INDEX]));
    }

    public String getDestination() {
        return destination;
    }

    public int getPrice() {
        return price;
    }
}
~~~
