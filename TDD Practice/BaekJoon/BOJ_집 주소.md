# 집 주소 (Bronze 3)

### 문제 설명

재석이는 대문에 붙이는 (주소를 나타내는) 호수판 제작업체의 직원이다. 고객에게 전달할 호수판은 숫자와 숫자 사이 그리고 왼쪽 오른쪽으로 적당히 여백이 들어가 줘야하고 숫자마다 차지하는 간격이 조금씩 상이하다. 다행이도 규칙은 매우 간단하다. 

각 숫자 사이에는 1cm의 여백이 들어가야한다.
1은 2cm의 너비를 차지해야한다. 0은 4cm의 너비를 차지해야한다. 나머지 숫자는 모두 3cm의 너비를 차지한다.
호수판의 경계와 숫자 사이에는 1cm의 여백이 들어가야한다.

<img src="https://upload.acmicpc.net/f203f2d5-ff6a-4afb-9cfe-226612dd4095/-/preview/" width="213">

예를 들어 위의 120 같은 경우,  각 숫자 사이에 여백이 1cm 씩 2개 들어간다. 1은 2cm, 2는 3cm, 0은 4cm를 차지한다. 오른쪽, 왼쪽 경계에서 각각 여백이 1cm씩 차지한다. 따라서 총 2 + 2 + 3 + 4 + 1 + 1 = 13(cm) 가 된다.

재석이는 고객에게 전달해야할 호수판의 너비가 얼마나 되는지 궁금해졌다. 재석이를 도와주자!

출처 : https://www.acmicpc.net/problem/1284

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String addressNumber = scanner.nextLine();
        
        while (!addressNumber.equals("0")) {
            AddressSign addressSign = new AddressSign();
            System.out.println(addressSign.getSizeOfAddressSign(addressNumber));
            addressNumber = scanner.nextLine();
        }
    }
}

class AddressSign {

    private static final String DELIMITER = "";
    private static final int SIDE_BLANK = 1;

    public int getSizeOfAddressSign(String addressNumber) {
        return getSizeOfAddressNumbers(addressNumber) + getSizeOfBlank(addressNumber);
    }

    private static int getSizeOfAddressNumbers(String addressNumber) {
        return Arrays.stream(addressNumber.split(DELIMITER))
            .mapToInt(AddressSign::getSizeOfAddressNumber)
            .sum();
    }

    private static int getSizeOfAddressNumber(String addressNumber) {
        return AddressNumberSize.from(addressNumber).getSize();
    }

    private static int getSizeOfBlank(String addressNumber) {
        return addressNumber.length() + SIDE_BLANK;
    }
}

enum AddressNumberSize {
    ONE("1", 2),
    ZERO("0", 4),
    OTHER("Other", 3);

    private final String addressNumber;
    private final int size;

    AddressNumberSize(String addressNumber, int size) {
        this.addressNumber = addressNumber;
        this.size = size;
    }

    public static AddressNumberSize from(String addressNumber) {
        return Arrays.stream(AddressNumberSize.values())
            .filter(addressNumberSize -> addressNumberSize.addressNumber.equals(addressNumber))
            .findFirst()
            .orElse(AddressNumberSize.OTHER);
    }

    public int getSize() {
        return size;
    }
}
~~~

---

#### 테스트 코드
~~~java
package test;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

import static org.junit.jupiter.api.Assertions.*;

import java.util.Arrays;
import java.util.stream.Stream;

public class MainTest {

    @ParameterizedTest
    @MethodSource("provideAddressSign")
    @DisplayName("주소 번호를 받아 해당 주소판 길이를 반환한다.")
    void shouldReturnSizeOfAddressSign(String addressNumber, int expected) {
        AddressSign sut = new AddressSign();
        
        int actual = sut.getSizeOfAddressSign(addressNumber);
        
        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideAddressSign() {
        return Stream.of(
            Arguments.of("3", 5),
            Arguments.of("1", 4),
            Arguments.of("0", 6),
            Arguments.of("33", 9),
            Arguments.of("13", 8),
            Arguments.of("10", 9),
            Arguments.of("00", 11),
            Arguments.of("120", 13),
            Arguments.of("5611", 15),
            Arguments.of("100", 14)
        );
    }
}

class AddressSign {

    private static final String DELIMITER = "";
    private static final int SIDE_BLANK = 1;

    public int getSizeOfAddressSign(String addressNumber) {
        return getSizeOfAddressNumbers(addressNumber) + getSizeOfBlank(addressNumber);
    }

    private static int getSizeOfAddressNumbers(String addressNumber) {
        return Arrays.stream(addressNumber.split(DELIMITER))
            .mapToInt(AddressSign::getSizeOfAddressNumber)
            .sum();
    }
    
    private static int getSizeOfAddressNumber(String addressNumber) {
        return AddressNumberSize.from(addressNumber).getSize();
    }

    private static int getSizeOfBlank(String addressNumber) {
        return addressNumber.length() + SIDE_BLANK;
    }
}

enum AddressNumberSize {
    ONE("1", 2),
    ZERO("0", 4),
    OTHER("Other", 3);

    private final String addressNumber;
    private final int size;
    
    AddressNumberSize(String addressNumber, int size) {
        this.addressNumber = addressNumber;
        this.size = size;
    }
    
    public static AddressNumberSize from(String addressNumber) {
        return Arrays.stream(AddressNumberSize.values())
            .filter(addressNumberSize -> addressNumberSize.addressNumber.equals(addressNumber))
            .findFirst()
            .orElse(AddressNumberSize.OTHER);
    }

    public int getSize() {
        return size;
    }
}
~~~
