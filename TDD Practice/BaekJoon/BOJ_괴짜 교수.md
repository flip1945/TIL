# 괴짜 교수 (Bronze 3)

### 문제 설명

승혁이는 괴짜 교수이다. 그는 미래에 컴퓨터 프로그램을 만들기 위해서는 컴퓨터 프로그램을 병렬로 만들어야 한다고 믿는다. 그가 옳다는 것을 확신시키기 위해서 그는 실험을 진행 하길 원했다. 

실험과정은 다음과 같다: 그는 먼저 몇 개의 문제에 대해 이 프로그램이 다음 해 동안 실행 될 횟수를 예상한다. 그리고 그는 그의 조교에게 병렬버전의 프로그램을 개발하고, 그 프로그램을 개발하는 데 걸리는 시간을 측정하라고 지시한다. 마지막으로,  그들은 병렬버전과 직렬버전의 실행 시간을 측정한다. 

이 측정된 데이터를 기반으로, 승혁이는 어떤 경우에 병렬화를 통해 전반적인 작업량을 최소화하는지 알고 싶어한다. 이 일에 대한 작업량은 병렬버전을 개발하는 시간과 그 프로그램이 실행될 때까지 기다리는 시간이다. 

출처 : https://www.acmicpc.net/problem/11109

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            ProgramSelector selector = new ProgramSelector(scanner.nextInt(), scanner.nextInt(), scanner.nextInt(),
                    scanner.nextInt());

            System.out.println(selector.select());
        }
    }
}

class ProgramSelector {

    public static final String DOES_NOT_MATTER = "does not matter";
    public static final String PREFER_SERIALIZE = "do not parallelize";
    public static final String PREFER_PARALLELIZE = "parallelize";

    private final int timeToDevelop;
    private final int runCount;
    private final int executionTimeOfSerialize;
    private final int executionTimeOfParallelize;

    public ProgramSelector(int timeToDevelop, int runCount, int executionTimeOfSerialize,
                           int executionTimeOfParallelize) {
        this.timeToDevelop = timeToDevelop;
        this.runCount = runCount;
        this.executionTimeOfSerialize = executionTimeOfSerialize;
        this.executionTimeOfParallelize = executionTimeOfParallelize;
    }

    public String select() {
        if (isSameTimeOfParallelizeAndSerialize()) {
            return DOES_NOT_MATTER;
        }
        return isPreferSerialize() ? PREFER_SERIALIZE : PREFER_PARALLELIZE;
    }

    private boolean isPreferSerialize() {
        return getTotalTimeOfSerialize() < getTotalTimeOfParallelize();
    }

    private boolean isSameTimeOfParallelizeAndSerialize() {
        return getTotalTimeOfSerialize() == getTotalTimeOfParallelize();
    }

    private int getTotalTimeOfSerialize() {
        return executionTimeOfSerialize * runCount;
    }

    private int getTotalTimeOfParallelize() {
        return timeToDevelop + (executionTimeOfParallelize * runCount);
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
    @MethodSource("provideProgramInfos")
    @DisplayName("병렬화가 좋으면 parallelize, 하지 않는 게 좋다면 do not parallelize, 상관 없다면 does not matter를 반환한다.")
    void testProgramSelect(int timeToDevelop, int runCount, int executionTimeOfSerialize,
                           int executionTimeOfParallelize, String expected) {
        ProgramSelector sut = new ProgramSelector(timeToDevelop, runCount, executionTimeOfSerialize,
                executionTimeOfParallelize);

        String actual = sut.select();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideProgramInfos() {
        return Stream.of(
                Arguments.of(1, 1, 1, 1, "do not parallelize"),
                Arguments.of(1, 1, 3, 1, "parallelize"),
                Arguments.of(1, 1, 2, 1, "does not matter"),
                Arguments.of(1, 2, 2, 1, "parallelize"),
                Arguments.of(10, 2, 3, 2, "do not parallelize"),
                Arguments.of(20, 5, 8, 2, "parallelize"),
                Arguments.of(0, 2, 1, 1, "does not matter")
        );
    }
}

class ProgramSelector {

    public static final String DOES_NOT_MATTER = "does not matter";
    public static final String PREFER_SERIALIZE = "do not parallelize";
    public static final String PREFER_PARALLELIZE = "parallelize";

    private final int timeToDevelop;
    private final int runCount;
    private final int executionTimeOfSerialize;
    private final int executionTimeOfParallelize;

    public ProgramSelector(int timeToDevelop, int runCount, int executionTimeOfSerialize,
                           int executionTimeOfParallelize) {
        this.timeToDevelop = timeToDevelop;
        this.runCount = runCount;
        this.executionTimeOfSerialize = executionTimeOfSerialize;
        this.executionTimeOfParallelize = executionTimeOfParallelize;
    }

    public String select() {
        if (isSameTimeOfParallelizeAndSerialize()) {
            return DOES_NOT_MATTER;
        }
        return isPreferSerialize() ? PREFER_SERIALIZE : PREFER_PARALLELIZE;
    }

    private boolean isPreferSerialize() {
        return getTotalTimeOfSerialize() < getTotalTimeOfParallelize();
    }

    private boolean isSameTimeOfParallelizeAndSerialize() {
        return getTotalTimeOfSerialize() == getTotalTimeOfParallelize();
    }

    private int getTotalTimeOfSerialize() {
        return executionTimeOfSerialize * runCount;
    }

    private int getTotalTimeOfParallelize() {
        return timeToDevelop + (executionTimeOfParallelize * runCount);
    }
}
~~~
