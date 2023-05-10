# 3대 측정 (Bronze 2)

### 문제 설명

웨이트 트레이닝에서의 3대 측정은 스쿼트, 벤치프레스, 데드리프트의 중량을 측정하는 것이다. 하지만 세 명이 한 팀을 이루어 출전하는 전국 대학생 프로그래밍 대회(ICPC)의 참가자들은 다소 독특한 방법으로 3대를 측정하는데, 바로 팀원 각각의 실력을 수치로 나타내 주는 ‘코드포스 레이팅’을 비교하는 것이다.

웨이트 트레이닝계에는 3대 중량을 합쳐 500kg를 넘지 못하는 사람은 ‘언더아머’ 브랜드의 옷을 입지 못한다는 암묵적인 룰이 있으며. 이들을 단속하는 ‘언더아머 단속반’이 존재한다는 소문도 있다. Sogang ICPC Team은 이를 벤치마킹해 팀원 3명의 코드포스 레이팅의 합이 
$K$ 미만인 팀은 가입할 수 없는 VIP 클럽을 만들고자 한다.

하지만 이런 조건에서는 세 명 중 한 명의 레이팅이 현저히 낮더라도 나머지 두 명의 레이팅이 충분히 높다면 세 명이 모두 VIP 클럽에 가입할 수 있게 된다. 클럽에 가입하는 사람들은 모두 일정 수준 이상이어야겠다고 판단한 학회장 임지환은 모든 팀원의 레이팅이 
$\boldsymbol{L}$ 이상이고, 팀원 세 명의 레이팅의 합이 
$\boldsymbol{K}$ 이상인 팀만이 가입할 수 있게 하였다.

학회 일과 연합 일에 치여 사는 지환이는 VIP 클럽의 회원 관리까지 할 시간이 없다. 지환이를 위해 지원자 중 VIP 클럽에 가입할 수 있는 팀의 수를 구하고, VIP 회원들의 레이팅을 출력해 보자.

출처 : https://www.acmicpc.net/problem/20299

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int k = Integer.parseInt(st.nextToken());
        int l = Integer.parseInt(st.nextToken());

        int countOfVipTeam = 0;
        StringBuilder vipRatings = new StringBuilder();
        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(br.readLine());
            int rating1 = Integer.parseInt(st.nextToken());
            int rating2 = Integer.parseInt(st.nextToken());
            int rating3 = Integer.parseInt(st.nextToken());
            List<Integer> inputTeamRating = inputTeamRating(rating1, rating2, rating3);
            TeamRating teamRating = new TeamRating(inputTeamRating);
            if (teamRating.isSatisfiedRating(k, l)) {
                countOfVipTeam++;
                vipRatings.append(rating1).append(" ")
                        .append(rating2).append(" ")
                        .append(rating3).append(" ");
            }
        }

        printVipRatings(countOfVipTeam, vipRatings);
    }

    private static List<Integer> inputTeamRating(int rating1, int rating2, int rating3) {
        return List.of(rating1, rating2, rating3);
    }

    private static void printVipRatings(int countOfVipTeam, StringBuilder vipRatings) {
        System.out.println(countOfVipTeam);
        System.out.println(vipRatings);
    }
}

class TeamRating {
    private final List<Integer> teamRating;

    public TeamRating(List<Integer> teamRating) {
        this.teamRating = teamRating;
    }

    public boolean isSatisfiedRating(int totalRatingLimit, int minRatingLimit) {
        return isSatisfiedTotalRating(totalRatingLimit) && isSatisfiedMinRating(minRatingLimit);
    }

    private boolean isSatisfiedTotalRating(int totalRatingLimit) {
        return getTotalRating() >= totalRatingLimit;
    }

    private int getTotalRating() {
        return teamRating.stream()
                .mapToInt(Integer::intValue)
                .sum();
    }

    private boolean isSatisfiedMinRating(int minRatingLimit) {
        return teamRating.stream()
                .allMatch(rating -> rating >= minRatingLimit);
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
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideTeamRatings")
    @DisplayName("레이팅을 만족하는 팀만 가입할 수 있다.")
    void only_team_that_satisfy_rating_can_join(List<Integer> teamRating, int totalRatingLimit, int minRatingLimit, boolean expected) {
        TeamRating sut = new TeamRating(teamRating);

        boolean actual = sut.isSatisfiedRating(totalRatingLimit, minRatingLimit);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideTeamRatings() {
        return Stream.of(
                Arguments.of(List.of(2, 2, 2), 4, 2, true),
                Arguments.of(List.of(2, 2, 2), 4, 3, false),
                Arguments.of(List.of(2, 2, 2), 7, 2, false),
                Arguments.of(List.of(1621, 1928, 1809), 5000, 1600, true),
                Arguments.of(List.of(2300, 2300, 1499), 5000, 1600, false),
                Arguments.of(List.of(1805, 1211, 1699), 5000, 1600, false),
                Arguments.of(List.of(1600, 1700, 1800), 5000, 1600, true),
                Arguments.of(List.of(1792, 1617, 1830), 5000, 1600, true),
                Arguments.of(List.of(0, 0, 0), 0, 0, true),
                Arguments.of(List.of(4000, 4000, 4000), 12000, 4000, true)
        );
    }
}

class TeamRating {
    private final List<Integer> teamRating;

    public TeamRating(List<Integer> teamRating) {
        this.teamRating = teamRating;
    }

    public boolean isSatisfiedRating(int totalRatingLimit, int minRatingLimit) {
        return isSatisfiedTotalRating(totalRatingLimit) && isSatisfiedMinRating(minRatingLimit);
    }

    private boolean isSatisfiedTotalRating(int totalRatingLimit) {
        return getTotalRating() >= totalRatingLimit;
    }

    private int getTotalRating() {
        return teamRating.stream()
                .mapToInt(Integer::intValue)
                .sum();
    }

    private boolean isSatisfiedMinRating(int minRatingLimit) {
        return teamRating.stream()
                .allMatch(rating -> rating >= minRatingLimit);
    }
}
~~~
