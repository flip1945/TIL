# 팀명 정하기 (Bronze 3)

### 문제 설명

> 현대 모비스는 모빌리티 SW 해커톤, 알고리즘 경진대회, 채용 연계형 SW 아카데미 등 다양한 SW 인재 발굴 프로그램을 진행하고 있다. 지난 2월에 개최된 모빌리티 SW 해커톤은 국내 14개 대학의 소프트웨어 동아리 20개 팀, 70여 명이 참여해 모빌리티 소프트웨어 개발 실력을 겨뤘다.

숭실대학교 컴퓨터학부 문제해결 소모임 SCCC 부원들은 매년 모빌리티 SW 해커톤, SCON, ICPC와 같은 팀 대회에서 사용할 팀명을 정하기 위해 많은 고민을 한다. 졸업을 한 학기 남겨둔 성서는 더 이상 부원들이 팀명으로 고통을 받지 않도록 가이드라인을 만들었다.

성서의 가이드라인에 따르면 팀 이름을 짓는 방법은 두 가지가 있다.

1. 세 참가자의 입학 연도를 100으로 나눈 나머지를 오름차순으로 정렬해서 이어 붙인 문자열
2. 세 참가자 중 성씨를 영문으로 표기했을 때의 첫 글자를 백준 온라인 저지에서 해결한 문제가 많은 사람부터 차례대로 나열한 문자열

예를 들어 600문제를 해결한 18학번 안(AHN)씨, 2000문제를 해결한 19학번 이(LEE)씨, 6000문제를 해결한 20학번 오(OH)씨로 구성된 팀을 생각해 보자. 첫 번째 방법으로 팀명을 만들면 181920이 되고, 두 번째 방법으로 팀명을 만들면 OLA가 된다.

2000문제를 해결한 19학번 이(LEE)씨, 9000문제를 21학번 나(NAH)씨, 1000문제를 해결한 22학번 박(PARK)씨로 구성된 팀은 첫 번째 방법으로 팀명을 만들면 192122가 되고, 두 번째 방법으로 팀명을 만들면 NLP가 된다.

세 팀원의 백준 온라인 저지에서 해결한 문제의 개수, 입학 연도, 그리고 성씨가 주어지면 첫 번째 방법과 두 번째 방법으로 만들어지는 팀명을 차례대로 출력하는 프로그램을 작성하라.

출처 : https://www.acmicpc.net/problem/28114

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Team team = Team.from(inputParticipants(scanner));
        System.out.println(team.createTeamName());
    }

    private static List<String> inputParticipants(Scanner scanner) {
        return IntStream.range(0, 3)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class Team {

    public static final String DELIMITER = "\n";
    private final List<Participant> participants;

    public Team(List<Participant> participants) {
        this.participants = participants;
    }

    public static Team from(List<String> participants) {
        return new Team(initParticipants(participants));
    }

    private static List<Participant> initParticipants(List<String> participants) {
        return participants.stream()
                .map(Participant::from)
                .collect(Collectors.toList());
    }

    public String createTeamName() {
        return createTeamNameOfYears() + DELIMITER + createTeamNameOfName();
    }

    private String createTeamNameOfYears() {
        return participants.stream()
                .map(Participant::getYearOfDecade)
                .sorted()
                .collect(Collectors.joining(""));
    }

    private String createTeamNameOfName() {
        return participants.stream()
                .sorted()
                .map(Participant::getInitial)
                .collect(Collectors.joining(""));
    }
}

class Participant implements Comparable<Participant> {

    private final int countOfSolved;
    private final String year;
    private final String name;

    private Participant(int countOfSolved, String year, String name) {
        this.countOfSolved = countOfSolved;
        this.year = year;
        this.name = name;
    }

    public static Participant from(String participantInfo) {
        String[] split = participantInfo.split(" ");
        return new Participant(Integer.parseInt(split[0]), split[1], split[2]);
    }

    public String getInitial() {
        return name.substring(0, 1);
    }

    public String getYearOfDecade() {
        return year.substring(2, 4);
    }

    @Override
    public int compareTo(Participant o) {
        return Integer.compare(o.countOfSolved, countOfSolved);
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Participant that = (Participant) o;
        return countOfSolved == that.countOfSolved && Objects.equals(year, that.year) && Objects.equals(name, that.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(countOfSolved, year, name);
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

import java.util.List;
import java.util.Objects;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideParticipants")
    @DisplayName("팀명을 정한다.")
    void testCreateTeamName(List<String> userInfos, String expected) {
        Team sut = Team.from(userInfos);

        String actual = sut.createTeamName();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideParticipants() {
        return Stream.of(
                Arguments.of(List.of("600 2018 AHN", "2000 2019 LEE", "6000 2020 OH"), "181920\nOLA"),
                Arguments.of(List.of("600 2020 A", "2000 2030 B", "6000 2040 C"), "203040\nCBA"),
                Arguments.of(List.of("1 2090 D", "2 2050 A", "3 2000 B"), "005090\nBAD"),
                Arguments.of(List.of("1000 2022 PARK", "9000 2021 NAH", "2000 2019 LEE"), "192122\nNLP")
        );
    }
}

class Team {

    public static final String DELIMITER = "\n";
    private final List<Participant> participants;

    public Team(List<Participant> participants) {
        this.participants = participants;
    }

    public static Team from(List<String> participants) {
        return new Team(initParticipants(participants));
    }

    private static List<Participant> initParticipants(List<String> participants) {
        return participants.stream()
                .map(Participant::from)
                .collect(Collectors.toList());
    }

    public String createTeamName() {
        return createTeamNameOfYears() + DELIMITER + createTeamNameOfName();
    }

    private String createTeamNameOfYears() {
        return participants.stream()
                .map(Participant::getYearOfDecade)
                .sorted()
                .collect(Collectors.joining(""));
    }

    private String createTeamNameOfName() {
        return participants.stream()
                .sorted()
                .map(Participant::getInitial)
                .collect(Collectors.joining(""));
    }
}

class Participant implements Comparable<Participant> {

    private final int countOfSolved;
    private final String year;
    private final String name;

    private Participant(int countOfSolved, String year, String name) {
        this.countOfSolved = countOfSolved;
        this.year = year;
        this.name = name;
    }

    public static Participant from(String participantInfo) {
        String[] split = participantInfo.split(" ");
        return new Participant(Integer.parseInt(split[0]), split[1], split[2]);
    }

    public String getInitial() {
        return name.substring(0, 1);
    }

    public String getYearOfDecade() {
        return year.substring(2, 4);
    }

    @Override
    public int compareTo(Participant o) {
        return Integer.compare(o.countOfSolved, countOfSolved);
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Participant that = (Participant) o;
        return countOfSolved == that.countOfSolved && Objects.equals(year, that.year) && Objects.equals(name, that.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(countOfSolved, year, name);
    }
}
~~~
