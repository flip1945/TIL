# 학번을 찾아줘! (Bronze 4)

### 문제 설명

나는 7번의 수능 끝에 한양대학교 23학번으로 입학하게 된 김한양이다. 한양대학교 추가합격 전화를 받고 기뻐서 친구들과 술을 마시며 길을 걷다가 언덕 아래로 굴러 떨어지고 말았다. 깨어나보니 2주가 흘러 있었고, 수강신청까지는 15분밖에 남지 않았다. 수강신청 홈페이지에 들어갔지만 부상의 후유증으로 학번이 기억나지 않았다. 한양대학교 공지사항을 확인해보니 학번을 생성하는 방법에 대한 안내문이 있었다.

한양대학교는 Engine of Korea라고 불리는 만큼 학번을 만들 때 특수한 공식을 사용한다. 기본적으로 한양대학교 학번을 알아내기 위해서는 정확한 수능 점수(표준 점수)를 알고 있어야 한다.

* 국어 점수가 영어 점수보다 높다면, 두 점수의 차에 인문관의 건물 번호 $508$을 곱해준다. 아니라면, 두 점수의 차에 국제관의 건물 번호 $108$을 곱해준다.
* 수학 점수가 탐구 점수보다 높다면, 두 점수의 차에 제1공학관의 건물 번호 $212$를 곱해준다. 아니라면, 두 점수의 차에 ITBT관의 건물 번호 $305$을 곱해준다.
* 제2외국어를 응시했다면 제2외국어 점수에 행원파크 건물 번호인 $707$을 곱해준다.
* 위에서 계산한 세 값을 더한 뒤, 한양대학교의 우편번호 $04763 (= 4763)$을 곱하면 학번이 나온다.

위의 계산을 하려던 김한양은 머리를 다쳐서 암산이 안 된다는 것을 깨닫고 당신에게 학번을 계산해주는 프로그램을 만들어 달라고 부탁하였다. 김한양의 학번을 계산해줄 프로그램을 만들어라! 단, 앞의 과목을 응시하지 않으면 뒤의 과목을 응시할 수 없는 구조이며, 응시하지 않은 과목의 표준점수는 $0$점이라고 가정하자.

출처 : https://www.acmicpc.net/problem/29807

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        List<Integer> scores = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());

        StudentId studentId = StudentId.from(scores);
        System.out.println(studentId.getId());
    }
}

class StudentId {

    private static final int UNIVERSITY_ZIPCODE = 4763;
    private static final int HUMANITIES_HALL_NUMBER = 508;
    private static final int INTERNATIONAL_HALL_NUMBER = 108;
    private static final int ENGINEERING_HALL_NUMBER = 212;
    private static final int ITBT_HALL_NUMBER = 305;
    private static final int HANGWON_PARK_NUMBER = 707;

    private final int koreanScore;
    private final int mathScore;
    private final int englishScore;
    private final int scienceScore;
    private final int secondForeignScore;

    public StudentId(int koreanScore, int mathScore, int englishScore, int scienceScore, int secondForeignScore) {
        this.koreanScore = koreanScore;
        this.mathScore = mathScore;
        this.englishScore = englishScore;
        this.scienceScore = scienceScore;
        this.secondForeignScore = secondForeignScore;
    }

    public static StudentId from(List<Integer> scores) {
        List<Integer> initScores = initScores(scores);
        return new StudentId(initScores.get(0), initScores.get(1), initScores.get(2), initScores.get(3), initScores.get(4));
    }

    private static List<Integer> initScores(List<Integer> scores) {
        List<Integer> result = new ArrayList<>(scores);
        for (int i = 0; i < 5 - scores.size(); i++) {
            result.add(0);
        }
        return result;
    }

    public int getId() {
        return (getFirstNumber() + getSecondNumber() + getThirdNumber()) * UNIVERSITY_ZIPCODE;
    }

    private int getFirstNumber() {
        int diff = Math.abs(koreanScore - englishScore);
        return (koreanScore > englishScore) ? diff * HUMANITIES_HALL_NUMBER : diff * INTERNATIONAL_HALL_NUMBER;
    }

    private int getSecondNumber() {
        int diff = Math.abs(mathScore - scienceScore);
        return (mathScore > scienceScore) ? diff * ENGINEERING_HALL_NUMBER : diff * ITBT_HALL_NUMBER;
    }

    private int getThirdNumber() {
        return secondForeignScore * HANGWON_PARK_NUMBER;
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

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSubjectScores")
    @DisplayName("과목 점수들이 주어지면 학번을 계산한다.")
    void studentIdCalculateStudentIdGivenSubjectScores(List<Integer> scores, int expected) {
        StudentId sut = StudentId.from(scores);

        int actual = sut.getId();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSubjectScores() {
        return Stream.of(
                Arguments.of(List.of(88, 92, 90, 90, 35), 120908755),
                Arguments.of(List.of(94, 96, 90, 88), 17756464),
                Arguments.of(List.of(50, 25, 25, 50), 96807975)
        );
    }
}

class StudentId {

    private static final int UNIVERSITY_ZIPCODE = 4763;
    private static final int HUMANITIES_HALL_NUMBER = 508;
    private static final int INTERNATIONAL_HALL_NUMBER = 108;
    private static final int ENGINEERING_HALL_NUMBER = 212;
    private static final int ITBT_HALL_NUMBER = 305;
    private static final int HANGWON_PARK_NUMBER = 707;

    private final int koreanScore;
    private final int mathScore;
    private final int englishScore;
    private final int scienceScore;
    private final int secondForeignScore;

    public StudentId(int koreanScore, int mathScore, int englishScore, int scienceScore, int secondForeignScore) {
        this.koreanScore = koreanScore;
        this.mathScore = mathScore;
        this.englishScore = englishScore;
        this.scienceScore = scienceScore;
        this.secondForeignScore = secondForeignScore;
    }

    public static StudentId from(List<Integer> scores) {
        List<Integer> initScores = initScores(scores);
        return new StudentId(initScores.get(0), initScores.get(1), initScores.get(2), initScores.get(3), initScores.get(4));
    }

    private static List<Integer> initScores(List<Integer> scores) {
        List<Integer> result = new ArrayList<>(scores);
        for (int i = 0; i < 5 - scores.size(); i++) {
            result.add(0);
        }
        return result;
    }

    public int getId() {
        return (getFirstNumber() + getSecondNumber() + getThirdNumber()) * UNIVERSITY_ZIPCODE;
    }

    private int getFirstNumber() {
        int diff = Math.abs(koreanScore - englishScore);
        return (koreanScore > englishScore) ? diff * HUMANITIES_HALL_NUMBER : diff * INTERNATIONAL_HALL_NUMBER;
    }

    private int getSecondNumber() {
        int diff = Math.abs(mathScore - scienceScore);
        return (mathScore > scienceScore) ? diff * ENGINEERING_HALL_NUMBER : diff * ITBT_HALL_NUMBER;
    }

    private int getThirdNumber() {
        return secondForeignScore * HANGWON_PARK_NUMBER;
    }
}
~~~
