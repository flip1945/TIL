# 회사에 있는 사람 (Silver 5)

### 문제 설명

상근이는 세계적인 소프트웨어 회사 기글에서 일한다. 이 회사의 가장 큰 특징은 자유로운 출퇴근 시간이다. 따라서, 직원들은 반드시 9시부터 6시까지 회사에 있지 않아도 된다.

각 직원은 자기가 원할 때 출근할 수 있고, 아무때나 퇴근할 수 있다.

상근이는 모든 사람의 출입카드 시스템의 로그를 가지고 있다. 이 로그는 어떤 사람이 회사에 들어왔는지, 나갔는지가 기록되어져 있다. 로그가 주어졌을 때, 현재 회사에 있는 모든 사람을 구하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/7785

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> logs = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());

        AttendanceManagementSystem system = new AttendanceManagementSystem();
        AttendanceRecorder recorder = new AttendanceRecorder(system);
        recorder.record(logs);

        system.whoIsInCompany().forEach(System.out::println);
    }
}

class AttendanceRecorder {

    private final AttendanceManagementSystem system;

    public AttendanceRecorder(AttendanceManagementSystem system) {
        this.system = system;
    }

    public void record(List<String> attendanceLogs) {
        attendanceLogs.stream()
                .map(AttendanceLog::from)
                .forEach(system::record);
    }
}

class AttendanceManagementSystem {

    private final Set<String> peopleInCompany = new TreeSet<>(Comparator.reverseOrder());

    public List<String> whoIsInCompany() {
        return new ArrayList<>(peopleInCompany);
    }

    public void record(AttendanceLog attendanceLog) {
        switch (attendanceLog.getState()) {
            case enter:
                peopleInCompany.add(attendanceLog.getName());
                break;
            case leave:
                peopleInCompany.remove(attendanceLog.getName());
                break;
            default:
                throw new IllegalStateException("시스템에 정의되지 않은 상태입니다.");
        }
    }
}

class AttendanceLog {

    public static final String LOG_DELIMITER = " ";
    public static final int NAME_INDEX = 0;
    public static final int STATE_INDEX = 1;

    private final String name;
    private final State state;

    private AttendanceLog(String name, State state) {
        this.name = name;
        this.state = state;
    }

    public static AttendanceLog from(String attendanceLog) {
        String[] log = attendanceLog.split(LOG_DELIMITER);
        return new AttendanceLog(log[NAME_INDEX], State.valueOf(log[STATE_INDEX]));
    }

    public String getName() {
        return name;
    }

    public State getState() {
        return state;
    }

    enum State {
        enter, leave;
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
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideAttendanceLogs")
    @DisplayName("출근관리시스템은 회사에 남아있는 사원의 명단을 보여준다.")
    void ams_shows_list_of_remaining_employees_in_company(List<String> attendanceLogs, List<String> expected) {
        AttendanceManagementSystem sut = new AttendanceManagementSystem();
        AttendanceRecorder recorder = new AttendanceRecorder(sut);
        recorder.record(attendanceLogs);

        List<String> actual = sut.whoIsInCompany();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideAttendanceLogs() {
        return Stream.of(
                Arguments.of(List.of("a enter"), List.of("a")),
                Arguments.of(List.of("a enter", "a leave"), List.of()),
                Arguments.of(List.of("a enter", "b enter"), List.of("b", "a")),
                Arguments.of(List.of("a enter", "b enter", "c enter"), List.of("c", "b", "a")),
                Arguments.of(List.of("a enter", "b enter", "c enter", "a leave", "b leave", "c leave"), List.of())
        );
    }
}

class AttendanceRecorder {

    private final AttendanceManagementSystem system;

    public AttendanceRecorder(AttendanceManagementSystem system) {
        this.system = system;
    }

    public void record(List<String> attendanceLogs) {
        attendanceLogs.stream()
                .map(AttendanceLog::from)
                .forEach(system::record);
    }
}

class AttendanceManagementSystem {

    private final Set<String> peopleInCompany = new TreeSet<>(Comparator.reverseOrder());

    public List<String> whoIsInCompany() {
        return new ArrayList<>(peopleInCompany);
    }

    public void record(AttendanceLog attendanceLog) {
        switch (attendanceLog.getState()) {
            case enter:
                peopleInCompany.add(attendanceLog.getName());
                break;
            case leave:
                peopleInCompany.remove(attendanceLog.getName());
                break;
            default:
                throw new IllegalStateException("시스템에 정의되지 않은 상태입니다.");
        }
    }
}

class AttendanceLog {

    public static final String LOG_DELIMITER = " ";
    public static final int NAME_INDEX = 0;
    public static final int STATE_INDEX = 1;

    private final String name;
    private final State state;

    private AttendanceLog(String name, State state) {
        this.name = name;
        this.state = state;
    }

    public static AttendanceLog from(String attendanceLog) {
        String[] log = attendanceLog.split(LOG_DELIMITER);
        return new AttendanceLog(log[NAME_INDEX], State.valueOf(log[STATE_INDEX]));
    }

    public String getName() {
        return name;
    }

    public State getState() {
        return state;
    }

    enum State {
        enter, leave;
    }
}
~~~
