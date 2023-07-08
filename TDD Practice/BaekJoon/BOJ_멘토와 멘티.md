# 멘토와 멘티 (Silver 5)

### 문제 설명

서울사이버대학교에는 멘토링 프로그램이 있다. 멘토링 프로그램은 한 명의 멘토(선배학습자)가 여러 명의 멘티(후배학습자)에게 대학 생활에 대한 노하우와 정보 등을 전수하는 것이다.

빅데이터·AI 센터에서 딥러닝 서버를 돌리며 바쁜 나날을 보내던 노교수는, 어느 날 멘토링 회의 참석 요청이 들어와 이를 준비하던 도중 멘토-멘티 순서쌍 목록이 적힌 노트를 찾았다. 하지만 노트가 제대로 정리되어 있지 않아 분석이 어려웠으므로, 마침 센터에서 인턴을 하는 대학원생 뚜루에게 목록의 정렬을 맡기기로 했다.

하지만 논문을 쓰느라 수면 부족에 시달리고 있는 뚜루는 이러한 프로그램을 짤 틈이 나지 않았기 때문에, 당신에게 다시 목록을 맡겼다. 목록이 주어질 때 멘토를 기준으로 사전 순으로 정렬하되, 멘토가 같은 순서쌍들에 대해선 멘티의 사전 역순으로 정렬하자.

출처 : https://www.acmicpc.net/problem/26265

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(bufferedReader.readLine());
        List<String> mentorAndMentees = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            mentorAndMentees.add(bufferedReader.readLine());
        }

        MentorMenteePairs mentorMenteePairs = MentorMenteePairs.from(mentorAndMentees);
        printMentorMenteePairs(mentorMenteePairs.getSortedPairs());
    }

    private static void printMentorMenteePairs(List<String> mentorMenteePairs) {
        mentorMenteePairs.forEach(System.out::println);
    }
}

class MentorMenteePairs {

    private final List<MentorMenteePair> mentorMenteePairs;

    private MentorMenteePairs(List<MentorMenteePair> mentorMenteePairs) {
        this.mentorMenteePairs = mentorMenteePairs;
    }

    public static MentorMenteePairs from(List<String> mentorAndMentees) {
        return new MentorMenteePairs(initMentorMenteePairs(mentorAndMentees));
    }

    private static List<MentorMenteePair> initMentorMenteePairs(List<String> mentorAndMentees) {
        return mentorAndMentees.stream()
                .map(MentorMenteePair::from)
                .collect(Collectors.toList());
    }

    public List<String> getSortedPairs() {
        return mentorMenteePairs.stream()
                .sorted()
                .map(MentorMenteePair::toString)
                .collect(Collectors.toList());
    }
}

class MentorMenteePair implements Comparable<MentorMenteePair> {

    public static final String DELIMITER = " ";
    public static final int MENTOR_INDEX = 0;
    public static final int MENTEE_INDEX = 1;

    private final String mentor;
    private final String mentee;

    private MentorMenteePair(String mentor, String mentee) {
        this.mentor = mentor;
        this.mentee = mentee;
    }

    public static MentorMenteePair from(String mentorAndMentee) {
        String[] split = mentorAndMentee.split(DELIMITER);
        return new MentorMenteePair(split[MENTOR_INDEX], split[MENTEE_INDEX]);
    }

    @Override
    public int compareTo(MentorMenteePair o) {
        int result = this.mentor.compareTo(o.mentor);
        if (result != 0) {
            return result;
        }
        return o.mentee.compareTo(this.mentee);
    }

    @Override
    public String toString() {
        return mentor + " " + mentee;
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
    @MethodSource("provideMentorAndMentees")
    @DisplayName("주어진 분수에 대해 맞는 대분수를 문자열로 반환한다.")
    void getMixedFractionTest(List<String> mentorAndMentees, List<String> expected) {
        MentorMenteePairs sut = MentorMenteePairs.from(mentorAndMentees);

        List<String> actual = sut.getSortedPairs();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideMentorAndMentees() {
        return Stream.of(
                Arguments.of(List.of("a ab", "b bb"), List.of("a ab", "b bb")),
                Arguments.of(List.of("b bb", "a ab"), List.of("a ab", "b bb")),
                Arguments.of(List.of("a ab", "b bb", "a ac"), List.of("a ac", "a ab", "b bb")),
                Arguments.of(List.of("alice bob", "alice dave", "eve mallory", "peggy victor", "alice carol"),
                        List.of("alice dave", "alice carol", "alice bob", "eve mallory", "peggy victor")),
                Arguments.of(List.of("cgiosy jhnah", "son kim", "son lee", "cgiosy leejseo", "cgiosy junseo", "son hwang", "cgiosy sean", "cgiosy cologne"),
                        List.of("cgiosy sean", "cgiosy leejseo", "cgiosy junseo", "cgiosy jhnah", "cgiosy cologne", "son lee", "son kim", "son hwang"))
        );
    }
}

class MentorMenteePairs {

    private final List<MentorMenteePair> mentorMenteePairs;

    private MentorMenteePairs(List<MentorMenteePair> mentorMenteePairs) {
        this.mentorMenteePairs = mentorMenteePairs;
    }

    public static MentorMenteePairs from(List<String> mentorAndMentees) {
        return new MentorMenteePairs(initMentorMenteePairs(mentorAndMentees));
    }

    private static List<MentorMenteePair> initMentorMenteePairs(List<String> mentorAndMentees) {
        return mentorAndMentees.stream()
                .map(MentorMenteePair::from)
                .collect(Collectors.toList());
    }

    public List<String> getSortedPairs() {
        return mentorMenteePairs.stream()
                .sorted()
                .map(MentorMenteePair::toString)
                .collect(Collectors.toList());
    }
}

class MentorMenteePair implements Comparable<MentorMenteePair> {

    public static final String DELIMITER = " ";
    public static final int MENTOR_INDEX = 0;
    public static final int MENTEE_INDEX = 1;
    
    private final String mentor;
    private final String mentee;

    private MentorMenteePair(String mentor, String mentee) {
        this.mentor = mentor;
        this.mentee = mentee;
    }

    public static MentorMenteePair from(String mentorAndMentee) {
        String[] split = mentorAndMentee.split(DELIMITER);
        return new MentorMenteePair(split[MENTOR_INDEX], split[MENTEE_INDEX]);
    }

    @Override
    public int compareTo(MentorMenteePair o) {
        int result = this.mentor.compareTo(o.mentor);
        if (result != 0) {
            return result;
        }
        return o.mentee.compareTo(this.mentee);
    }

    @Override
    public String toString() {
        return mentor + " " + mentee;
    }
}
~~~
