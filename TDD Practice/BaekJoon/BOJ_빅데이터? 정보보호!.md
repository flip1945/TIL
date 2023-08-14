# 빅데이터? 정보보호! (Bronze 3)

### 문제 설명

서울사이버대학교 빅데이터·정보보호학과는 빅데이터에 관심이 있는 학생들과 정보보호에 관심이 있는 학생들이 골고루 섞여 있는 학과이다.

빅데이터·정보보호학과에서 수업을 하던 노교수는 학생들이 빅데이터와 정보보호 중 어느 분야에 더 관심이 많은지 궁금해졌다. 그래서 학생들을 만날 때마다 항상 이를 물어보고 답을 bigdata 혹은 security로 구분하여 메모장에 적어두었는데, 실수로 띄어쓰기와 개행이 전혀 없는 상태로 기록해두었다.

이대로는 학생들이 빅데이터와 정보보호 중 어느 분야에 더 관심이 많은지를 알아낼 수 없기 때문에, 당신에게 분석을 의뢰했다. 물어본 학생의 수와 답이 주어질 때, 결과를 출력하자.

출처 : https://www.acmicpc.net/problem/26264

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();
        BigDataSecurity bigDataSecurity = new BigDataSecurity();
        System.out.println(bigDataSecurity.check(scanner.nextLine()));
    }
}

class BigDataSecurity {

    public static final String BIGDATA = "bigdata";
    public static final String SECURITY = "security";
    public static final String BIGDATA_SECURITY_RESULT = "bigdata? security!";
    public static final String BIGDATA_RESULT = "bigdata?";
    public static final String SECURITY_RESULT = "security!";
    public static final String REPLACEMENT = "";

    public String check(String surveyResults) {
        return checkSurveyResults(countOf(surveyResults, BIGDATA), countOf(surveyResults, SECURITY));
    }

    private String checkSurveyResults(int countOfBigdata, int countOfSecurity) {
        if (countOfBigdata == countOfSecurity) {
            return BIGDATA_SECURITY_RESULT;
        }
        return countOfBigdata > countOfSecurity ?  BIGDATA_RESULT : SECURITY_RESULT;
    }

    private int countOf(String originString, String target) {
        return (originString.length() - originString.replace(target, REPLACEMENT).length()) / target.length();
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

import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSurveyResults")
    @DisplayName("설문 조사 결과가 주어졌을 때 더 관심이 많은 분야를 반환한다.")
    void testCheckBigDataOrSecurity(String surveyResults, String expected) {
        BigDataSecurity sut = new BigDataSecurity();

        String actual = sut.check(surveyResults);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSurveyResults() {
        return Stream.of(
                Arguments.of("bigdata", "bigdata?"),
                Arguments.of("security", "security!"),
                Arguments.of("securitybigdata", "bigdata? security!"),
                Arguments.of("bigdatasecuritybigdata", "bigdata?"),
                Arguments.of("securitybigdatasecuritybigdatasecurity", "security!"),
                Arguments.of("bigdatabigdatabigdatasecuritysecuritysecurity", "bigdata? security!")
        );
    }
}

class BigDataSecurity {

    public static final String BIGDATA = "bigdata";
    public static final String SECURITY = "security";
    public static final String BIGDATA_SECURITY_RESULT = "bigdata? security!";
    public static final String BIGDATA_RESULT = "bigdata?";
    public static final String SECURITY_RESULT = "security!";
    public static final String REPLACEMENT = "";

    public String check(String surveyResults) {
        return checkSurveyResults(countOf(surveyResults, BIGDATA), countOf(surveyResults, SECURITY));
    }

    private String checkSurveyResults(int countOfBigdata, int countOfSecurity) {
        if (countOfBigdata == countOfSecurity) {
            return BIGDATA_SECURITY_RESULT;
        }
        return countOfBigdata > countOfSecurity ?  BIGDATA_RESULT : SECURITY_RESULT;
    }

    private int countOf(String originString, String target) {
        return (originString.length() - originString.replace(target, REPLACEMENT).length()) / target.length();
    }
}
~~~
