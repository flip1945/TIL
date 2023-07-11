# Java Bitecode (Bronze 2)

### 문제 설명

> 어서 내 손을  JAVA~

태한이는 JAVA를 싫어한다. 매우 싫어한다. 아주 앙증맞게 깨물고 싶을 정도다.

그래서 태한이는 코딩을 할 때 알파벳 J, A, V는 사용하지 않는다. 또한 기존의 코드에서도 J, A, V가 보이면 전부 이빨로 깨물어 제거한다. 기존의 코드에서 J, A, V를 깨물어 제거한 코드를 **JAVA Bitecode**라고 부른다.

입력으로 길이가 
$N$인 코드 
$S$가 주어지면, 그 코드의 **JAVA Bitecode**를 구해보자!

출처 : https://www.acmicpc.net/problem/21867

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();
        
        BiteCode biteCode = BiteCode.from(scanner.nextLine());
        System.out.println(biteCode);
    }
}

class BiteCode {

    public static final String NO_JAVA_CODE = "nojava";

    private final String biteCode;

    private BiteCode(String biteCode) {
        this.biteCode = removeJavaCode(biteCode);
    }

    private String removeJavaCode(String code) {
        return code.replaceAll("[JAV]", "");
    }

    public static BiteCode from(String code) {
        return new BiteCode(code);
    }

    @Override
    public String toString() {
        return getBiteCodeOrNoJavaCode();
    }

    private String getBiteCodeOrNoJavaCode() {
        return biteCode.length() == 0 ? NO_JAVA_CODE : biteCode;
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
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideCodes")
    @DisplayName("주어진 코드에서 J, A, V를 제거한 코드를 반환한다. 길이가 0이라면 nojava를 반환한다.")
    void convertJavaBiteCodeTest(String code, String expected) {
        BiteCode sut = BiteCode.from(code);

        String actual = sut.toString();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideCodes() {
        return Stream.of(
                Arguments.of("AB", "B"),
                Arguments.of("ABC", "BC"),
                Arguments.of("AVC", "C"),
                Arguments.of("JAVO", "O"),
                Arguments.of("ABCD", "BCD"),
                Arguments.of("JAVA", "nojava")
        );
    }
}

class BiteCode {

    public static final String NO_JAVA_CODE = "nojava";

    private final String biteCode;

    private BiteCode(String biteCode) {
        this.biteCode = removeJavaCode(biteCode);
    }

    private String removeJavaCode(String code) {
        return code.replaceAll("[JAV]", "");
    }

    public static BiteCode from(String code) {
        return new BiteCode(code);
    }

    @Override
    public String toString() {
        return getBiteCodeOrNoJavaCode();
    }

    private String getBiteCodeOrNoJavaCode() {
        return biteCode.length() == 0 ? NO_JAVA_CODE : biteCode;
    }
}
~~~
