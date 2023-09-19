# 브실이의 입시전략 (Silver 5)

### 문제 설명

올해 고3인 브실이는 세계 최고의 명문 대학 브실대학(브론즈실버대학)에 가기 위해서 자신의 현재 점수를 토대로 입시 전략을 세우려고 한다. 브실대학에서는 특정 과목들의 성적의 합을 통해 서류 전형의 합격여부를 결정한다고 한다. 그러나 브실대학에서는 어떤 과목이 서류 평가에 반영되는지 모두 알려주지 않고 일부만 알려주는 사악한 학교다. 브실대학에서 요구하는 과목 수와 반영된다고 공개된 과목들이 주어질 때, 브실이가 얻을 수 있는 최소 점수와 최대 점수를 구해보자.

단, 공개된 과목과 비공개된 과목은 브실이가 수강한 과목에 모두 포함되어 있으며, 과목은 중복되지 않는다.

출처 : https://www.acmicpc.net/problem/29723

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        int k = Integer.parseInt(st.nextToken());

        List<String> courseInfos = inputList(br, n);
        Set<String> publicSubjects = inputSet(br, k);

        University university = new University(courseInfos, m, publicSubjects);
        System.out.println(university.getMinScoreAndMaxScore());
    }

    private static List<String> inputList(BufferedReader br, int n) throws IOException {
        List<String> result = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            result.add(br.readLine());
        }
        return result;
    }

    private static Set<String> inputSet(BufferedReader br, int n) throws IOException {
        Set<String> result = new HashSet<>();
        for (int i = 0; i < n; i++) {
            result.add(br.readLine());
        }
        return result;
    }
}

class University {

    private static final String DELIMITER = " ";

    private final List<String> courseInfos;
    private final int requiredCourses;
    private final Set<String> publicSubjects;

    public University(List<String> courseInfos, int requiredCourses, Set<String> publicSubjects) {
        this.courseInfos = courseInfos;
        this.requiredCourses = requiredCourses;
        this.publicSubjects = publicSubjects;
    }

    public String getMinScoreAndMaxScore() {
        return getMinScore() + DELIMITER + getMaxScore();
    }

    private int getMinScore() {
        return getPublicScore() + calculateMinScore();
    }

    private int getPublicScore() {
        return courseInfos.stream()
                .map(Course::from)
                .filter(course -> publicSubjects.contains(course.getName()))
                .mapToInt(Course::getScore)
                .sum();
    }

    private int calculateMinScore() {
        List<Integer> scoresWithoutPublic = getScoresWithoutPublic();
        return IntStream.range(0, getRequiredCoursesWithoutPublic())
                .map(scoresWithoutPublic::get)
                .sum();
    }

    private int getRequiredCoursesWithoutPublic() {
        return requiredCourses - publicSubjects.size();
    }

    private List<Integer> getScoresWithoutPublic() {
        return courseInfos.stream()
                .map(Course::from).filter(course -> !publicSubjects.contains(course.getName()))
                .map(Course::getScore)
                .sorted()
                .collect(Collectors.toList());
    }

    private int getMaxScore() {
        return getPublicScore() + calculateMaxScore();
    }

    private int calculateMaxScore() {
        List<Integer> scoresWithoutPublic = getScoresWithoutPublic();
        return IntStream.range(0, getRequiredCoursesWithoutPublic())
                .map(i -> scoresWithoutPublic.get(scoresWithoutPublic.size() - 1 - i))
                .sum();
    }
}

class Course {

    private static final String DELIMITER = " ";
    private static final int NAME_INDEX = 0;
    private static final int SCORE_INDEX = 1;

    private final String name;
    private final int score;

    private Course(String name, int score) {
        this.name = name;
        this.score = score;
    }

    public static Course from(String courseInfo) {
        return new Course(courseInfo.split(DELIMITER)[NAME_INDEX],
                Integer.parseInt(courseInfo.split(DELIMITER)[SCORE_INDEX]));
    }

    public String getName() {
        return name;
    }

    public int getScore() {
        return score;
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
    @MethodSource("provideSubjectsAndPoints")
    @DisplayName("대학은 얻을 수 있는 최소 점수와 최대 점수를 계산한다.")
    void universityCalculateMinScoreAndMaxScore(List<String> courseInfos, int requiredCourses, Set<String> publicSubjects, String expected) {
        University sut = new University(courseInfos, requiredCourses, publicSubjects);

        String actual = sut.getMinScoreAndMaxScore();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSubjectsAndPoints() {
        return Stream.of(
                Arguments.of(List.of("a 100"), 1, Set.of("a"), "100 100"),
                Arguments.of(List.of("a 50"), 1, Set.of("a"), "50 50"),
                Arguments.of(List.of("a 50", "b 100"), 1, Set.of("b"), "100 100"),
                Arguments.of(List.of("a 50", "b 100"), 2, Set.of("a", "b"), "150 150"),
                Arguments.of(List.of("a 50", "b 100", "c 70"), 2, Set.of("a"), "120 150"),
                Arguments.of(List.of("a 50", "b 100", "c 70", "d 10"), 2, Set.of("a"), "60 150"),
                Arguments.of(List.of("a 50", "b 100", "c 70", "d 10"), 3, Set.of("a"), "130 220"),
                Arguments.of(List.of("a 100", "b 70", "c 50", "d 80", "e 90", "f 100"), 3, Set.of("c", "e"), "210 240")
        );
    }
}

class University {

    private static final String DELIMITER = " ";

    private final List<String> courseInfos;
    private final int requiredCourses;
    private final Set<String> publicSubjects;

    public University(List<String> courseInfos, int requiredCourses, Set<String> publicSubjects) {
        this.courseInfos = courseInfos;
        this.requiredCourses = requiredCourses;
        this.publicSubjects = publicSubjects;
    }

    public String getMinScoreAndMaxScore() {
        return getMinScore() + DELIMITER + getMaxScore();
    }

    private int getMinScore() {
        return getPublicScore() + calculateMinScore();
    }

    private int getPublicScore() {
        return courseInfos.stream()
                .map(Course::from)
                .filter(course -> publicSubjects.contains(course.getName()))
                .mapToInt(Course::getScore)
                .sum();
    }

    private int calculateMinScore() {
        List<Integer> scoresWithoutPublic = getScoresWithoutPublic();
        return IntStream.range(0, getRequiredCoursesWithoutPublic())
                .map(scoresWithoutPublic::get)
                .sum();
    }

    private int getRequiredCoursesWithoutPublic() {
        return requiredCourses - publicSubjects.size();
    }

    private List<Integer> getScoresWithoutPublic() {
        return courseInfos.stream()
                .map(Course::from).filter(course -> !publicSubjects.contains(course.getName()))
                .map(Course::getScore)
                .sorted()
                .collect(Collectors.toList());
    }

    private int getMaxScore() {
        return getPublicScore() + calculateMaxScore();
    }

    private int calculateMaxScore() {
        List<Integer> scoresWithoutPublic = getScoresWithoutPublic();
        return IntStream.range(0, getRequiredCoursesWithoutPublic())
                .map(i -> scoresWithoutPublic.get(scoresWithoutPublic.size() - 1 - i))
                .sum();
    }
}

class Course {

    private static final String DELIMITER = " ";
    private static final int NAME_INDEX = 0;
    private static final int SCORE_INDEX = 1;

    private final String name;
    private final int score;

    private Course(String name, int score) {
        this.name = name;
        this.score = score;
    }

    public static Course from(String courseInfo) {
        return new Course(courseInfo.split(DELIMITER)[NAME_INDEX],
                Integer.parseInt(courseInfo.split(DELIMITER)[SCORE_INDEX]));
    }

    public String getName() {
        return name;
    }

    public int getScore() {
        return score;
    }
}
~~~
