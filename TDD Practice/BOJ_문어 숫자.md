# 문어 숫자 (Bronze 2)

### 문제 설명

해류가 매우 느리고 바닥을 기어다니는 생물이 적은 바다 밑바닥에서만 발견되는 잔물결 무늬의 정체는 오랫동안 해양학자들에게 수수께끼였다. 하지만 최근의 연구 성과는 동물 언어학 분야에 일대 혁명을 불러왔다. 이 무늬의 정체는 바로 문어가 숫자를 적는 방법이라는 것이 해양 생물학자들에 의해 밝혀진 것이다. 학자들은 문어가 무엇을 세는 것인지는 아직 알 수 없지만, 수 표기법을 해독하는 데에는 성공했다.

뭍 위에 사는 이들에게는 문어가 쓰는 숫자와 그를 표현하는 잔물결 무늬가 매우 낯설 수밖에 없다. 따라서 연구자들은 다음과 같은 기호로 잔물결 무늬를 적기로 합의했다. 각 기호와 대응하는 숫자는 다음과 같다.

* -는 0에 대응한다.
* \는 1에 대응한다.
* (는 2에 대응한다.
* @는 3에 대응한다.
* ?는 4에 대응한다.
* \>는 5에 대응한다.
* &는 6에 대응한다.
* %는 7에 대응한다.
* /는 -1에 대응한다.

해양 신경학자들은 특히 음수를 나타내는 기호가 있다는 사실에 흥분하면서, 아직 걸음마 단계인 두족류 신경학이 이 발견을 계기로 크게 발전하기를 기대하고 있다.

당연히 문어의 수 체계는 8진법에 기반한다. 예를 들면 다음과 같다.

(@&는 2 × 82 + 3 × 8 + 6 = 158이다.

?/--는 4 × 83 + −1 × 82 + 0 × 8 + 0 = 1984이다.
/(\는 −1 × 82 + 2 × 8 + 1 = −47이다.

당신에게 주어진 문제는 문어 숫자를 입력 받아 십진수로 나타내는 것이다.

출처 : https://www.acmicpc.net/problem/1864

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    private static final Map<String, Integer> octopusNumberMap = Map.of("-", 0, "\\", 1, "(", 2, "@", 3, "?", 4, ">", 5, "&", 6, "%", 7, "/", -1);

    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));

        String octopusString;
        while(!(octopusString = bf.readLine()).equals("#")) {
            System.out.println(convertOctopusStringToDecimal(octopusString));
        }
    }

    private static int convertOctopusStringToDecimal(String octopusString) {
        List<Integer> octalNumbers = new ArrayList<>();
        for (char s : octopusString.toCharArray()) {
            octalNumbers.add(convertOctal(String.valueOf(s)));
        }
        return convertDecimalInt(octalNumbers);
    }

    private static int convertOctal(String octopusNumber) {
        return octopusNumberMap.get(octopusNumber);
    }

    private static int convertDecimalInt(List<Integer> octalNumbers) {
        Collections.reverse(octalNumbers);
        int result = 0;
        for (int i = 0; i < octalNumbers.size(); i++) {
            result += octalNumbers.get(i) * Math.pow(8, i);
        }
        return result;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.*;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {
    private static final Map<String, Integer> octopusNumberMap = Map.of("-", 0, "\\", 1, "(", 2, "@", 3, "?", 4, ">", 5, "&", 6, "%", 7, "/", -1);

    @Test
    void octopusMapTest() {
        String zero = "-";
        String one = "\\";
        String two = "(";
        String three = "@";
        String four = "?";
        String five = ">";
        String six = "&";
        String seven = "%";
        String minus = "/";

        assertEquals(0, convertOctal(zero));
        assertEquals(1, convertOctal(one));
        assertEquals(2, convertOctal(two));
        assertEquals(3, convertOctal(three));
        assertEquals(4, convertOctal(four));
        assertEquals(5, convertOctal(five));
        assertEquals(6, convertOctal(six));
        assertEquals(7, convertOctal(seven));
        assertEquals(-1, convertOctal(minus));
    }

    @Test
    void octalTest() {
        List<Integer> octalList1 = Arrays.asList(2, 3, 6);
        List<Integer> octalList2 = Arrays.asList(1, 4);
        List<Integer> octalList3 = Arrays.asList(4, -1, 0, 0);
        List<Integer> octalList4 = Arrays.asList(-1, 2, 1);

        assertEquals(158, convertDecimalInt(octalList1));
        assertEquals(12, convertDecimalInt(octalList2));
        assertEquals(1984, convertDecimalInt(octalList3));
        assertEquals(-47, convertDecimalInt(octalList4));
    }

    @Test
    void integrationTest() {
        String octopusString1 = "(@&";
        String octopusString2 = "?/--";
        String octopusString3 = "/(\\";
        String octopusString4 = "?";

        assertEquals(158, convertOctopusStringToDecimal(octopusString1));
        assertEquals(1984, convertOctopusStringToDecimal(octopusString2));
        assertEquals(-47, convertOctopusStringToDecimal(octopusString3));
        assertEquals(4, convertOctopusStringToDecimal(octopusString4));
    }

    private int convertOctopusStringToDecimal(String octopusString) {
        List<Integer> octalNumbers = new ArrayList<>();
        for (char s : octopusString.toCharArray()) {
            octalNumbers.add(convertOctal(String.valueOf(s)));
        }
        return convertDecimalInt(octalNumbers);
    }

    private int convertOctal(String octopusNumber) {
        return octopusNumberMap.get(octopusNumber);
    }

    private int convertDecimalInt(List<Integer> octalNumbers) {
        Collections.reverse(octalNumbers);
        int result = 0;
        for (int i = 0; i < octalNumbers.size(); i++) {
            result += octalNumbers.get(i) * Math.pow(8, i);
        }
        return result;
    }
}
~~~
