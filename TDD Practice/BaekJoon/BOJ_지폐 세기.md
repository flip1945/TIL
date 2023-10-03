# 지폐 세기 (Bronze 4)

### 문제 설명

대한민국 지폐는 천 원권, 오천 원권, 만 원권, 오만 원권으로 총 네 종류가 있다. 각 지폐의 세로 길이는 $68\text{mm}$로 모두 같지만, 가로 길이는 모두 다르다. 천 원권의 가로 길이는 $136\text{mm}$, 오천 원권의 가로 길이는 $142\text{mm}$, 만 원권의 가로 길이는 $148\text{mm}$, 오만 원권의 가로 길이는 $154\text{mm}$이다. 따라서 가로의 길이를 통해서 지폐의 종류를 구분할 수 있다.

수민이는 대한민국 지폐 $N$장을 가지고 있다. 수민이는 종이의 크기를 재는 기계를 이용하여 각 지폐의 가로, 세로 길이를 알아냈다. 수민이가 가진 지폐의 총액을 구해보자.

출처 : https://www.acmicpc.net/problem/30031

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        Bills bills = Bills.from(inputBillInfos(scanner, n));
        System.out.println(bills.sum());
    }

    private static List<String> inputBillInfos(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Bills {

    private static final String DELIMITER = " ";
    private static final int LENGTH_INDEX = 0;

    private final List<Bill> bills;

    public Bills(List<Bill> bills) {
        this.bills = bills;
    }

    public static Bills from(List<String> billInfos) {
        List<Bill> result = initBills(billInfos);
        return new Bills(result);
    }

    private static List<Bill> initBills(List<String> billInfos) {
        return billInfos.stream()
                .map(billInfo -> Integer.parseInt(billInfo.split(DELIMITER)[LENGTH_INDEX]))
                .map(Bill::from)
                .collect(Collectors.toList());
    }

    public int sum() {
        return bills.stream()
                .mapToInt(Bill::getPrice)
                .sum();
    }
}

enum Bill {
    KRW_1000(1000, 136),
    KRW_5000(5000, 142),
    KRW_10000(10000, 148),
    KRW_50000(50000, 154),
    NONE(0, 0);

    private static final Map<Integer, Bill> lengthToBill = new HashMap<>();

    static {
        for (Bill bill : values()) {
            lengthToBill.put(bill.length, bill);
        }
    }

    private final int price;
    private final int length;

    Bill(int price, int length) {
        this.price = price;
        this.length = length;
    }

    public static Bill from(int length) {
        return lengthToBill.getOrDefault(length, NONE);
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

import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideBillLength")
    @DisplayName("지폐의 길이가 주어지면 가격을 반환한다.")
    void billReturnsPriceGivenLengthOfBill(int length, int expected) {
        Bill sut = Bill.from(length);

        int actual = sut.getPrice();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideBillLength() {
        return Stream.of(
                Arguments.of(136, 1000),
                Arguments.of(142, 5000),
                Arguments.of(148, 10000),
                Arguments.of(154, 50000),
                Arguments.of(1, 0)
        );
    }

    @ParameterizedTest
    @MethodSource("provideBillInfos")
    @DisplayName("지폐의 길이들이 주어지면 지폐의 총액을 반환한다.")
    void billsReturnsSumOfPricesGivenLengthsOfBills(List<String> lengths, int expected) {
        Bills sut = Bills.from(lengths);

        int actual = sut.sum();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideBillInfos() {
        return Stream.of(
                Arguments.of(List.of("136 68"), 1000),
                Arguments.of(List.of("136 68", "136 68"), 2000),
                Arguments.of(List.of("142 68"), 5000),
                Arguments.of(List.of("136 68", "142 68", "148 68", "154 68"), 66000)
        );
    }
}

class Bills {

    private static final String DELIMITER = " ";
    private static final int LENGTH_INDEX = 0;

    private final List<Bill> bills;

    public Bills(List<Bill> bills) {
        this.bills = bills;
    }

    public static Bills from(List<String> billInfos) {
        List<Bill> result = initBills(billInfos);
        return new Bills(result);
    }

    private static List<Bill> initBills(List<String> billInfos) {
        return billInfos.stream()
                .map(billInfo -> Integer.parseInt(billInfo.split(DELIMITER)[LENGTH_INDEX]))
                .map(Bill::from)
                .collect(Collectors.toList());
    }

    public int sum() {
        return bills.stream()
                .mapToInt(Bill::getPrice)
                .sum();
    }
}

enum Bill {
    KRW_1000(1000, 136),
    KRW_5000(5000, 142),
    KRW_10000(10000, 148),
    KRW_50000(50000, 154),
    NONE(0, 0);

    private static final Map<Integer, Bill> lengthToBill = new HashMap<>();

    static {
        for (Bill bill : values()) {
            lengthToBill.put(bill.length, bill);
        }
    }

    private final int price;
    private final int length;

    Bill(int price, int length) {
        this.price = price;
        this.length = length;
    }

    public static Bill from(int length) {
        return lengthToBill.getOrDefault(length, NONE);
    }

    public int getPrice() {
        return price;
    }
}
~~~
