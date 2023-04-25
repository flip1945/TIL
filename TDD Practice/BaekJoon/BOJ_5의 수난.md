# 5의 수난 (Bronze 2)

### 문제 설명

키파는 문득 3과 4의 견고한 벽에 가로막혀 스포트라이트를 받지 못하는 5를 떠올렸다. '세상에 얼마나 많은 것들이 5와 관련이 있는데!'

키파는 5가 쓰이는 곳을 떠올리기 시작했다. 사람의 손가락도 5개, 정다면체의 개수도 5개, 알려진 불가촉 홀수는 5뿐이고, 별은 보통 오각별, 그리고 무엇보다 "별이 다섯 개!"

그러자 문득 키파는 자신의 마음 속에서 다섯제곱을 하고 싶은 욕망이 올라오는 것을 느꼈다. 키파를 위해, 다섯 자리 수를 입력받아, 각 자릿수의 다섯제곱의 합을 출력하는 프로그램을 작성해 주자.

출처 : https://www.acmicpc.net/problem/23037

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(getSumOf5thPowers(scanner.nextLine()));
    }

    static int getSumOf5thPowers(String number) {
        return Arrays.stream(number.split(""))
                .map(Integer::parseInt)
                .mapToInt(i -> (int) Math.pow(i, 5))
                .sum();
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

import java.util.Arrays;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNumbers")
    @DisplayName("각 자리수의 다섯제곱의 합을 반환해야 한다.")
    void getSumOf5thPowersTest(String number, int expected) {
        int actual = getSumOf5thPowers(number);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNumbers() {
        return Stream.of(
                Arguments.of("12345", 4425),
                Arguments.of("54748", 54748),
                Arguments.of("92727", 92727),
                Arguments.of("93084", 93084),
                Arguments.of("10000", 1),
                Arguments.of("99999", 295245)
        );
    }

    private int getSumOf5thPowers(String number) {
        return Arrays.stream(number.split(""))
                .map(Integer::parseInt)
                .mapToInt(i -> (int) Math.pow(i, 5))
                .sum();
    }
}
~~~
