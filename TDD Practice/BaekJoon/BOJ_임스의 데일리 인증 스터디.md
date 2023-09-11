# 임스의 데일리 인증 스터디 (Silver 3)

### 문제 설명

취업 준비생 임스는 취업 준비를 하면서 그날그날 무슨 공부를 하였는지 기록하기 위해 데일리 인증이라는 스터디를 시작했다. 임스는 매일 무슨 공부를 하였는지 적으면서 몇 개의 규칙을 정했다.

* 매일 꾸준히 백준 문제를 푼다.
* 백준 문제를 하루 $1$문제 이상 풀었고, 그 외의 다른 공부는 $0$개 이상 진행하였다. 다른 공부들은 영어 대소문자, 숫자, 공백으로만 이루어진 최대 길이 $100$의 문자열이다.
* 인증 기록으로는 백준 문제 링크를 제일 마지막에 작성하고, 그 외 학습 기록은 문자열 길이가 짧은 순으로 정렬해서 작성한다. 만약 문자열의 길이가 같다면, 사전 순으로 정렬한다.
* 문자는 아스키코드 기준으로 비교한다.
* 백준 문제 링크는 boj.kr/문제 번호 형식이다. 문제 번호가 작은 순서대로 정렬해서 작성한다. 문제 번호는 $1$ 이상 $30\,000$ 이하이다.

임스가 하루 동안 공부한 기록들이 정렬되지 않은 채로 주어졌을 때, 주어진 규칙에 맞게 정렬 후 출력한다.

출처 : https://www.acmicpc.net/problem/29730

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());

        DailyStudyRecords records = new DailyStudyRecords(inputRecords(scanner, n));

        printRecords(records);
    }

    private static List<String> inputRecords(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }

    private static void printRecords(DailyStudyRecords records) {
        records.getSortedRecords().forEach(System.out::println);
    }
}

class DailyStudyRecords {

    private final List<String> dailyStudyRecords;

    public DailyStudyRecords(List<String> dailyStudyRecords) {
        this.dailyStudyRecords = dailyStudyRecords;
    }

    public List<String> getSortedRecords() {
        return mergeRecords(getSortedStudyRecords(dailyStudyRecords), getSortedBojStudyRecords(dailyStudyRecords));
    }

    private List<String> mergeRecords(List<String> firstRecords, List<String> secondRecords) {
        return Stream.concat(firstRecords.stream(), secondRecords.stream())
                .collect(Collectors.toList());
    }

    private List<String> getSortedStudyRecords(List<String> records) {
        return records.stream()
                .filter(record -> !isBojStudyRecord(record))
                .sorted(Comparator.comparingInt(String::length).thenComparing(o -> o))
                .collect(Collectors.toList());
    }

    private List<String> getSortedBojStudyRecords(List<String> records) {
        return records.stream()
                .filter(this::isBojStudyRecord)
                .sorted(Comparator.comparingInt(String::length).thenComparing(o -> o))
                .collect(Collectors.toList());
    }

    private boolean isBojStudyRecord(String record) {
        return record.startsWith("boj.kr/");
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

import java.util.Comparator;
import java.util.List;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideDailyRecords")
    @DisplayName("공부한 기록을 규칙에 맞게 정렬해야 한다.")
    void shouldSortDailyStudyRecords(List<String> dailyStudyRecords, List<String> expected) {
        DailyStudyRecords sut = new DailyStudyRecords(dailyStudyRecords);

        List<String> actual = sut.getSortedRecords();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideDailyRecords() {
        return Stream.of(
                Arguments.of(List.of("read book", "boj.kr/1"), List.of("read book", "boj.kr/1")),
                Arguments.of(List.of("read book", "boj.kr/100"), List.of("read book", "boj.kr/100")),
                Arguments.of(List.of("boj.kr/100", "read book"), List.of("read book", "boj.kr/100")),
                Arguments.of(List.of("boj.kr/100"), List.of("boj.kr/100")),
                Arguments.of(List.of("a", "b", "boj.kr/100"), List.of("a", "b", "boj.kr/100")),
                Arguments.of(List.of("a", "c", "b", "boj.kr/100"), List.of("a", "b", "c", "boj.kr/100")),
                Arguments.of(List.of("da", "e", "boj.kr/100"), List.of("e", "da", "boj.kr/100")),
                Arguments.of(List.of("da", "e", "boj.kr/10", "boj.kr/2"), List.of("e", "da", "boj.kr/2", "boj.kr/10")),
                Arguments.of(List.of("boj.kr/10", "boj.kr/1", "boj.kr/2"), List.of("boj.kr/1", "boj.kr/2", "boj.kr/10")),
                Arguments.of(List.of("boj.kr/1307", "study gc", "read book"), List.of("study gc", "read book", "boj.kr/1307"))
        );
    }
}

class DailyStudyRecords {

    private final List<String> dailyStudyRecords;

    public DailyStudyRecords(List<String> dailyStudyRecords) {
        this.dailyStudyRecords = dailyStudyRecords;
    }

    public List<String> getSortedRecords() {
        return mergeRecords(getSortedStudyRecords(dailyStudyRecords), getSortedBojStudyRecords(dailyStudyRecords));
    }

    private List<String> mergeRecords(List<String> firstRecords, List<String> secondRecords) {
        return Stream.concat(firstRecords.stream(), secondRecords.stream())
                .collect(Collectors.toList());
    }

    private List<String> getSortedStudyRecords(List<String> records) {
        return records.stream()
                .filter(record -> !isBojStudyRecord(record))
                .sorted(Comparator.comparingInt(String::length).thenComparing(o -> o))
                .collect(Collectors.toList());
    }

    private List<String> getSortedBojStudyRecords(List<String> records) {
        return records.stream()
                .filter(this::isBojStudyRecord)
                .sorted(Comparator.comparingInt(String::length).thenComparing(o -> o))
                .collect(Collectors.toList());
    }

    private boolean isBojStudyRecord(String record) {
        return record.startsWith("boj.kr/");
    }
}
~~~
