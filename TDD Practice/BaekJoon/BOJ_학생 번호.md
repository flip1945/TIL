# 학생 번호 (Silver 4)

### 문제 설명
이번에는 학생들을 더욱 효율적으로 관리하기 위해 학생마다 고유한 학생 번호를 부여하기로 하였다. 학생 번호는 0부터 9 사이의 숫자로 이루어진 문자열로, 모든 학생들의 학생 번호는 서로 다르지만 그 길이는 모두 같다.

학생들의 번호를 부여해 놓고 보니, 김진영 조교는 어쩌면 번호가 지나치게 긴 것은 아닌가 싶은 생각이 들었다. 예를 들어 아래와 같은 7자리의 학생 번호를 보자.

|이름|	번호|
|-|-|
|오민식|	1212345|
|김형택|	1212356|
|이동호|	0033445|

이처럼 학생 번호를 굳이 7자리로 하지 않고, 뒤에서 세 자리만을 추려서 남겨 놓아도 모든 학생들의 학생 번호를 서로 다르게 만들 수 있다.

|이름|	번호|
|-|-|
|오민식|	345|
|김형택|	356|
|이동호|	445|

하지만 세 자리보다 적게 남겨 놓아서는 모든 학생들의 학생 번호를 서로 다르게 만들 수 없다.

학생들의 번호가 주어 졌을 때, 뒤에서 k자리만을 추려서 남겨 놓았을 때 모든 학생들의 학생 번호를 서로 다르게 만들 수 있는 최소의 k를 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1235

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> studentNumbers = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());

        System.out.println(StudentNumberUtil.getMinIdentifiableNumberFromTail(studentNumbers));
    }
}

class StudentNumberUtil {
    public static int getMinIdentifiableNumberFromTail(List<String> studentNumbers) {
        return IntStream.range(1, getLengthOfStudentNumber(studentNumbers))
                .filter(indexFromTail -> canIdentify(studentNumbers, indexFromTail))
                .findFirst()
                .orElseGet(() -> getLengthOfStudentNumber(studentNumbers));
    }

    private static int getLengthOfStudentNumber(List<String> studentNumbers) {
        return studentNumbers.get(0).length();
    }

    private static boolean canIdentify(List<String> studentNumbers, int indexFromTail) {
        return getIdentifiableNumberSize(studentNumbers, indexFromTail) == studentNumbers.size();
    }

    private static int getIdentifiableNumberSize(List<String> studentNumbers, int indexFromTail) {
        Set<String> result = new HashSet<>();
        for (String studentNumber : studentNumbers) {
            String substring = studentNumber.substring(getLengthOfStudentNumber(studentNumbers) - indexFromTail);
            result.add(substring);
        }
        return result.size();
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
import java.util.Set;
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideStudentNumbers")
    @DisplayName("식별할 수 있는 최소 학생번호(뒷자리부터) 길이를 반환한다.")
    void getMinIdentifiableNumberFromTailTest(List<String> studentNumbers, int expected) {
        int actual = StudentNumberUtil.getMinIdentifiableNumberFromTail(studentNumbers);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideStudentNumbers() {
        return Stream.of(
                Arguments.of(List.of("01", "02", "03"), 1),
                Arguments.of(List.of("11", "21"), 2),
                Arguments.of(List.of("111", "112"), 1),
                Arguments.of(List.of("111", "121"), 2),
                Arguments.of(List.of("111", "211"), 3),
                Arguments.of(List.of("1212345", "1212356", "0033445"), 3)
        );
    }
}

class StudentNumberUtil {
    public static int getMinIdentifiableNumberFromTail(List<String> studentNumbers) {
        return IntStream.range(1, getLengthOfStudentNumber(studentNumbers))
                .filter(indexFromTail -> canIdentify(studentNumbers, indexFromTail))
                .findFirst()
                .orElseGet(() -> getLengthOfStudentNumber(studentNumbers));
    }

    private static int getLengthOfStudentNumber(List<String> studentNumbers) {
        return studentNumbers.get(0).length();
    }

    private static boolean canIdentify(List<String> studentNumbers, int indexFromTail) {
        return getIdentifiableNumberSize(studentNumbers, indexFromTail) == studentNumbers.size();
    }

    private static int getIdentifiableNumberSize(List<String> studentNumbers, int indexFromTail) {
        Set<String> result = new HashSet<>();
        for (String studentNumber : studentNumbers) {
            String substring = studentNumber.substring(getLengthOfStudentNumber(studentNumbers) - indexFromTail);
            result.add(substring);
        }
        return result.size();
    }
}
~~~
