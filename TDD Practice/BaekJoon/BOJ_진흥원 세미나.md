# 진흥원 세미나 (Bronze 5)

### 문제 설명

한국정보기술진흥원에서는 다양한 세미나가 열린다.

전문가를 위한 알고리즘, 데이터분석, 인공지능, 사이버보안, 네트워크 세미나뿐만 아니라 예비 창업가를 위한 창업 세미나, 그리고 청소년들을 위한 입시 세미나가 열린다.

오늘은 위 $7$개 주제의 세미나가 모두 열리는 날이다. 진흥이는 이 중 $N$ ($1 \le N \le 7$) 개의 세미나를 신청했다. 아래의 표를 보고 어느 교실에서 열리는지 알아보자.

|세미나|	교실|
|-|-|
|Algorithm|	204|
|DataAnalysis|	207|
|ArtificialIntelligence|	302|
|CyberSecurity|	B101|
|Network|	303|
|Startup|	501|
|TestStrategy|	105|

출처 : https://www.acmicpc.net/problem/30087

---

#### 풀이
~~~java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        for (int i = 0; i < n; i++) {
            Seminar seminar = Seminar.from(scanner.nextLine());
            System.out.println(seminar.getClassRoom());
        }
    }
}

class Seminar {

    private final SeminarType seminar;

    private Seminar(SeminarType seminar) {
        this.seminar = seminar;
    }

    public static Seminar from(String seminarName) {
        return new Seminar(SeminarType.from(seminarName));
    }

    public String getClassRoom() {
        return seminar.getClassRoom();
    }
}

enum SeminarType {
    ALGORITHM("Algorithm", "204"),
    DATA_ANALYSIS("DataAnalysis", "207"),
    ARTIFICIAL_INTELLIGENCE("ArtificialIntelligence", "302"),
    CYBER_SECURITY("CyberSecurity", "B101"),
    NETWORK("Network", "303"),
    STARTUP("Startup", "501"),
    TEST_STRATEGY("TestStrategy", "105");

    private final String seminarName;
    private final String classRoom;

    SeminarType(String seminarName, String classRoom) {
        this.seminarName = seminarName;
        this.classRoom = classRoom;
    }

    public static SeminarType from(String seminarName) {
        for (SeminarType seminarType : values()) {
            if (seminarType.seminarName.equals(seminarName)) {
                return seminarType;
            }
        }
        throw new IllegalArgumentException();
    }

    public String getClassRoom() {
        return classRoom;
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
    @MethodSource("provideSeminarName")
    @DisplayName("세미나는 세미나가 열리는 교실을 안다.")
    void seminarKnowsTheClassroom(String seminarName, String expected) {
        Seminar sut = Seminar.from(seminarName);

        String actual = sut.getClassRoom();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSeminarName() {
        return Stream.of(
                Arguments.of("Algorithm", "204"),
                Arguments.of("DataAnalysis", "207"),
                Arguments.of("ArtificialIntelligence", "302"),
                Arguments.of("CyberSecurity", "B101"),
                Arguments.of("Network", "303"),
                Arguments.of("Startup", "501"),
                Arguments.of("TestStrategy", "105")
        );
    }
}

class Seminar {

    private final SeminarType seminar;

    private Seminar(SeminarType seminar) {
        this.seminar = seminar;
    }

    public static Seminar from(String seminarName) {
        return new Seminar(SeminarType.from(seminarName));
    }

    public String getClassRoom() {
        return seminar.getClassRoom();
    }
}

enum SeminarType {
    ALGORITHM("Algorithm", "204"),
    DATA_ANALYSIS("DataAnalysis", "207"),
    ARTIFICIAL_INTELLIGENCE("ArtificialIntelligence", "302"),
    CYBER_SECURITY("CyberSecurity", "B101"),
    NETWORK("Network", "303"),
    STARTUP("Startup", "501"),
    TEST_STRATEGY("TestStrategy", "105");

    private final String seminarName;
    private final String classRoom;

    SeminarType(String seminarName, String classRoom) {
        this.seminarName = seminarName;
        this.classRoom = classRoom;
    }

    public static SeminarType from(String seminarName) {
        for (SeminarType seminarType : values()) {
            if (seminarType.seminarName.equals(seminarName)) {
                return seminarType;
            }
        }
        throw new IllegalArgumentException();
    }

    public String getClassRoom() {
        return classRoom;
    }
}
~~~
