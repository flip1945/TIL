# 와글와글 숭고한 (Bronze 4)

### 문제 설명

숭고한 알고리즘 캠프가 다가오고 있고 방학이 되어서까지도 각 대학들의 협업은 계속되고 있다. 그럼에도 불구하고 운영진들과 강사진들이 각자의 일정 때문에 바빠 계획에 차질이 조금씩 생기고 있다. 숭고한 알고리즘 캠프의 대표인 창호는 효율적인 일처리를 위해 엄정한 평가를 내리기로 하였다.

창호는 숭고한 알고리즘 캠프의 구성원인 숭실대학교(Soongsil University), 고려대학교(Korea University), 한양대학교(Hanyang University)의 참여도를 수치화하였다. 창호가 보기에 세 대학교의 참여도의 합이 100 이상이면 일처리가 잘 되고 있기에 안심할 수 있지만, 100 미만이면 창호는 참여도가 가장 낮은 대학의 동아리에게 무언의 압박을 넣을 예정이다. 숭고한 알고리즘 캠프의 성공을 빌며 창호의 고민을 해결해주자.

출처 : https://www.acmicpc.net/problem/17388

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Integer> rates = List.of(scanner.nextInt(), scanner.nextInt(), scanner.nextInt());
        
        AlgorithmCamp sut = AlgorithmCamp.from(rates);
        System.out.println(sut.checkEngagementRates());
    }
}

class AlgorithmCamp {

    private static final String OK = "OK";
    private static final int OK_THRESHOLD_RATE = 100;

    private final List<University> universities;

    private AlgorithmCamp(List<University> universities) {
        this.universities = universities;
    }

    public static AlgorithmCamp from(List<Integer> rates) {
        return new AlgorithmCamp(initUniversityOfRates(rates));
    }

    private static List<University> initUniversityOfRates(List<Integer> rates) {
        return List.of(
            new University(UniversityInfo.SOONGSIL, rates.get(UniversityInfo.SOONGSIL.getIndexOfRates())),
            new University(UniversityInfo.KOREA, rates.get(UniversityInfo.KOREA.getIndexOfRates())),
            new University(UniversityInfo.HANYANG, rates.get(UniversityInfo.HANYANG.getIndexOfRates()))
        );
    }

    public String checkEngagementRates() {
        if (getSumOfEngagementRates() >= OK_THRESHOLD_RATE) {
            return OK;
        }
        return getMinEngagementRateUniversity().getName();
    }

    private int getSumOfEngagementRates() {
        return universities.stream()
            .mapToInt(University::getEngagementRate)
            .sum();
    }

    private University getMinEngagementRateUniversity() {
        return universities.stream()
            .sorted()
            .findFirst()
            .orElseThrow(IllegalArgumentException::new);
    }
}

class University implements Comparable<University> {

    private final UniversityInfo universityInfo;
    private final int engagementRate;

    public University(UniversityInfo universityInfo, int engagementRate) {
        this.universityInfo = universityInfo;
        this.engagementRate = engagementRate;
    }

    @Override
    public int compareTo(University o) {
        return Integer.compare(this.engagementRate, o.engagementRate);
    }

    public String getName() {
        return universityInfo.getUniversityName();
    }

    public int getEngagementRate() {
        return engagementRate;
    }
}

enum UniversityInfo {
    SOONGSIL("Soongsil", 0),
    KOREA("Korea", 1),
    HANYANG("Hanyang", 2);

    private final String universityName;
    private final int indexOfRates;

    UniversityInfo(String universityName, int indexOfRates) {
        this.universityName = universityName;
        this.indexOfRates = indexOfRates;
    }

    public String getUniversityName() {
        return universityName;
    }

    public int getIndexOfRates() {
        return indexOfRates;
    }
}
~~~

---

#### 테스트 코드
~~~java
import static org.junit.jupiter.api.Assertions.*;

import java.util.List;
import java.util.stream.Stream;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideEngagementRates")
    @DisplayName("각 학교의 참여율을 확인해야 한다.")
    void shouldCheckEngagementRatesOfUniversities(List<Integer> rates, String expected) {
        AlgorithmCamp sut = AlgorithmCamp.from(rates);

        String actual = sut.checkEngagementRates();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideEngagementRates() {
        return Stream.of(
            Arguments.of(List.of(30, 40, 50), "OK"),
            Arguments.of(List.of(5, 40, 50), "Soongsil"),
            Arguments.of(List.of(40, 5, 50), "Korea"),
            Arguments.of(List.of(40, 50, 5), "Hanyang"),
            Arguments.of(List.of(19, 8, 8), "Korea"),
            Arguments.of(List.of(45, 33, 21), "Hanyang"),
            Arguments.of(List.of(3, 2, 1), "Hanyang"),
            Arguments.of(List.of(1, 2, 3), "Soongsil"),
            Arguments.of(List.of(2, 1, 3), "Korea"),
            Arguments.of(List.of(33, 34, 35), "OK"),
            Arguments.of(List.of(20, 30, 50), "OK"),
            Arguments.of(List.of(31, 41, 59), "OK")
        );
    }
}

class AlgorithmCamp {

    private static final String OK = "OK";
    private static final int OK_THRESHOLD_RATE = 100;

    private final List<University> universities;

    private AlgorithmCamp(List<University> universities) {
        this.universities = universities;
    }

    public static AlgorithmCamp from(List<Integer> rates) {
        return new AlgorithmCamp(initUniversityOfRates(rates));
    }

    private static List<University> initUniversityOfRates(List<Integer> rates) {
        return List.of(
            new University(UniversityInfo.SOONGSIL, rates.get(UniversityInfo.SOONGSIL.getIndexOfRates())),
            new University(UniversityInfo.KOREA, rates.get(UniversityInfo.KOREA.getIndexOfRates())),
            new University(UniversityInfo.HANYANG, rates.get(UniversityInfo.HANYANG.getIndexOfRates()))
        );
    }

    public String checkEngagementRates() {
        if (getSumOfEngagementRates() >= OK_THRESHOLD_RATE) {
            return OK;
        }
        return getMinEngagementRateUniversity().getName();
    }

    private int getSumOfEngagementRates() {
        return universities.stream()
            .mapToInt(University::getEngagementRate)
            .sum();
    }

    private University getMinEngagementRateUniversity() {
        return universities.stream()
            .sorted()
            .findFirst()
            .orElseThrow(IllegalArgumentException::new);
    }
}

class University implements Comparable<University> {

    private final UniversityInfo universityInfo;
    private final int engagementRate;

    public University(UniversityInfo universityInfo, int engagementRate) {
        this.universityInfo = universityInfo;
        this.engagementRate = engagementRate;
    }

    @Override
    public int compareTo(University o) {
        return Integer.compare(this.engagementRate, o.engagementRate);
    }

    public String getName() {
        return universityInfo.getUniversityName();
    }

    public int getEngagementRate() {
        return engagementRate;
    }
}

enum UniversityInfo {
    SOONGSIL("Soongsil", 0),
    KOREA("Korea", 1),
    HANYANG("Hanyang", 2);

    private final String universityName;
    private final int indexOfRates;

    UniversityInfo(String universityName, int indexOfRates) {
        this.universityName = universityName;
        this.indexOfRates = indexOfRates;
    }

    public String getUniversityName() {
        return universityName;
    }

    public int getIndexOfRates() {
        return indexOfRates;
    }
}
~~~
