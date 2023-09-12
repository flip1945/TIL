# 브실이와 친구가 되고 싶어 🤸‍♀️ (Bronze 4)

### 문제 설명

<img src="https://upload.acmicpc.net/a16efaf0-0a63-4d9a-98f2-368c045f888d/-/preview/" width="400px">

브실이의 친구들이 다 GOSU가 되자 자신과 실력이 비슷한 새로운 친구들을 사귀려고 한다. 주변을 아무리 둘러봐도 전부 GOSU뿐인 이곳에서 브실이는 겨우겨우 자신과 실력이 비슷한 사람들을 모았다. 놀랍게도 이 사람들이 푼 문제 수는 전부 다르며, 푼 문제 수가 적은 순서대로 배열했을 시 각각 $A, A+1, \cdots, B$ 문제를 풀었다고 한다.

새로운 친구를 사귈 생각에 들떠 있던 브실이는 이내 차가운 현실과 마주쳤다. 이들 중 몇 명은 브실이와 푼 문제 차이가 너무 많이 났기 때문이다. 그래서 브실이는 본인과 푼 문제 수 차이가 $X$보다 작거나 같은 사람만 친구의 자격을 갖추었다고 생각한다. 브실이가 푼 문제 수가 $K$개일 때, 브실이의 친구가 될 수 있는 사람의 수를 구해보자!

출처 : https://www.acmicpc.net/problem/29736

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ProblemSolvedFriends problemSolvedFriends = new ProblemSolvedFriends(scanner.nextInt(), scanner.nextInt(),
                scanner.nextInt(), scanner.nextInt());

        System.out.println(problemSolvedFriends.getCount());
    }
}

class ProblemSolvedFriends {

    private static final String NO_FRIENDS = "IMPOSSIBLE";
    private static final int NO_FRIENDS_COUNT = 0;

    private final int minSolvedCount;
    private final int maxSolvedCount;
    private final int countOfProblemSolved;
    private final int friendLimit;

    public ProblemSolvedFriends(int minSolvedCount, int maxSolvedCount, int countOfProblemSolved, int friendLimit) {
        this.minSolvedCount = minSolvedCount;
        this.maxSolvedCount = maxSolvedCount;
        this.countOfProblemSolved = countOfProblemSolved;
        this.friendLimit = friendLimit;
    }

    public String getCount() {
        int result = getCountOfFriends();
        return (result > NO_FRIENDS_COUNT) ? String.valueOf(result) : NO_FRIENDS;
    }

    private int getCountOfFriends() {
        return (int) IntStream.rangeClosed(minSolvedCount, maxSolvedCount)
                .filter(this::isFriend)
                .count();
    }

    private boolean isFriend(int solvedCount) {
        return Math.abs(countOfProblemSolved - solvedCount) <= friendLimit;
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
    @MethodSource("provideProblemSolvedInfo")
    @DisplayName("친구가 될 수 있는 사람의 수를 반환한다.")
    void shouldGetCountOfFriends(int minSolvedCount, int maxSolvedCount, int countOfProblemSolved, int friendLimit, String expected) {
        ProblemSolvedFriends sut = new ProblemSolvedFriends(minSolvedCount, maxSolvedCount, countOfProblemSolved, friendLimit);

        String actual = sut.getCount();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideProblemSolvedInfo() {
        return Stream.of(
                Arguments.of(1, 1, 3, 1, "IMPOSSIBLE"),
                Arguments.of(1, 1, 1, 0, "1"),
                Arguments.of(1, 30, 5, 0, "1"),
                Arguments.of(10, 100, 50, 10, "21"),
                Arguments.of(25, 75, 10, 5, "IMPOSSIBLE"),
                Arguments.of(10000, 10000, 10000, 10000, "1"),
                Arguments.of(1, 10000, 10000, 10000, "10000")
        );
    }
}

class ProblemSolvedFriends {

    private static final String NO_FRIENDS = "IMPOSSIBLE";
    private static final int NO_FRIENDS_COUNT = 0;

    private final int minSolvedCount;
    private final int maxSolvedCount;
    private final int countOfProblemSolved;
    private final int friendLimit;

    public ProblemSolvedFriends(int minSolvedCount, int maxSolvedCount, int countOfProblemSolved, int friendLimit) {
        this.minSolvedCount = minSolvedCount;
        this.maxSolvedCount = maxSolvedCount;
        this.countOfProblemSolved = countOfProblemSolved;
        this.friendLimit = friendLimit;
    }

    public String getCount() {
        int result = getCountOfFriends();
        return (result > NO_FRIENDS_COUNT) ? String.valueOf(result) : NO_FRIENDS;
    }

    private int getCountOfFriends() {
        return (int) IntStream.rangeClosed(minSolvedCount, maxSolvedCount)
                .filter(this::isFriend)
                .count();
    }

    private boolean isFriend(int solvedCount) {
        return Math.abs(countOfProblemSolved - solvedCount) <= friendLimit;
    }
}
~~~
