# 다중 항목 선호도 조사 (Small) (Bronze 1)

### 문제 설명

n명의 학생에게 다음과 같이 선호도를 조사하였다. 각 학생은 아래 세 가지 조사 항목 각각에 대하여 반드시 1가지를 선택해야 한다.

* 좋아하는 과목(subject)에 'kor', 'eng', 'math' 중 하나를 선택
* 좋아하는 과일(fruit)에 'apple', 'pear', 'orange' 중 하나를 선택
* 좋아하는 색깔(color)에 'red', 'blue', 'green' 중 하나를 선택

n명 학생의 선호도에 대하여 m개의 질의를 순서대로 처리하자. 하나의 질의는 다음과 같다.

* 질의 형식은 'subject fruit color'이다.
* subject은 'kor', 'eng', 'math', '-' 중 하나이다.
* fruit은 'apple', 'pear', 'orange', '-' 중 하나이다.
* color는 'red', 'blue', 'green', '-' 중 하나이다.
* '-' 표시는 해당 조건을 고려하지 않겠다는 의미이다.
* n명 학생 중 선호도가 subject fruit color와 일치하는 학생 수를 출력한다.

n명 학생의 선호도와 m개의 질의가 주어 지면, m개의 질의를 순서대로 처리하자.

출처 : https://www.acmicpc.net/problem/25326

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        Survey survey = new Survey();
        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(scanner.nextLine());
            survey.addSurveyResult(st.nextToken(), st.nextToken(), st.nextToken());
        }

        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(scanner.nextLine());
            System.out.println(survey.getMatchedCount(st.nextToken(), st.nextToken(), st.nextToken()));
        }
    }
}

class Survey {

    private final Map<SurveyKey, Integer> results = new HashMap<>();

    public void addSurveyResult(String subject, String fruit, String color) {
        SurveyKeys surveyKeys = new SurveyKeys(new SurveyKey(subject, fruit, color));
        for (SurveyKey surveyKey : surveyKeys.getSurveyKeys()) {
            results.merge(surveyKey, 1, Integer::sum);
        }
    }

    public int getMatchedCount(String subject, String fruit, String color) {
        return results.getOrDefault(new SurveyKey(subject, fruit, color), 0);
    }
}

class SurveyKeys {

    private static final String WILDCARD = "-";

    private final SurveyKey surveyKey;

    public SurveyKeys(SurveyKey surveyKey) {
        this.surveyKey = surveyKey;
    }

    public List<SurveyKey> getSurveyKeys() {
        return List.of(
                new SurveyKey(WILDCARD, WILDCARD, WILDCARD),
                new SurveyKey(WILDCARD, WILDCARD, surveyKey.getColor()),
                new SurveyKey(WILDCARD, surveyKey.getFruit(), WILDCARD),
                new SurveyKey(surveyKey.getSubject(), WILDCARD, WILDCARD),
                new SurveyKey(WILDCARD, surveyKey.getFruit(), surveyKey.getColor()),
                new SurveyKey(surveyKey.getSubject(), WILDCARD, surveyKey.getColor()),
                new SurveyKey(surveyKey.getSubject(), surveyKey.getFruit(), WILDCARD),
                new SurveyKey(surveyKey.getSubject(), surveyKey.getFruit(), surveyKey.getColor())
        );
    }
}

class SurveyKey {

    private final String subject;
    private final String fruit;
    private final String color;

    public SurveyKey(String subject, String fruit, String color) {
        this.subject = subject;
        this.fruit = fruit;
        this.color = color;
    }

    public String getSubject() {
        return subject;
    }

    public String getFruit() {
        return fruit;
    }

    public String getColor() {
        return color;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        SurveyKey surveyKey = (SurveyKey) o;
        return Objects.equals(subject, surveyKey.subject) && Objects.equals(fruit, surveyKey.fruit) && Objects.equals(color, surveyKey.color);
    }

    @Override
    public int hashCode() {
        return Objects.hash(subject, fruit, color);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Objects;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @Test
    @DisplayName("설문조사 결과가 없으면 0건을 반환한다.")
    void surveyReturnsZeroGivenZeroSurveyResult() {
        Survey sut = new Survey();

        int actual = sut.getMatchedCount("kor", "apple", "red");

        assertEquals(0, actual);
    }

    @Test
    @DisplayName("설문조사 결과 중 매칭되는 건이 하나 있으면 1건을 반환한다.")
    void surveyReturnsOneGivenOneMatchedSurveyResult() {
        Survey sut = new Survey();
        sut.addSurveyResult("kor", "apple", "red");

        int actual = sut.getMatchedCount("kor", "apple", "red");

        assertEquals(1, actual);
    }

    @Test
    @DisplayName("설문조사 결과 중 매칭되는 건이 두 개 있으면 2건을 반환한다.")
    void surveyReturnsTwoGivenTwoMatchedSurveyResult() {
        Survey sut = new Survey();
        sut.addSurveyResult("kor", "apple", "red");
        sut.addSurveyResult("kor", "apple", "red");

        int actual = sut.getMatchedCount("kor", "apple", "red");

        assertEquals(2, actual);
    }

    @Test
    @DisplayName("설문조사 결과 중 매칭되는 건의 개수를 반환한다.")
    void surveyReturnsOneGivenOneMatchedSurveyResultAndNotMatchedSurveyResult() {
        Survey sut = new Survey();
        sut.addSurveyResult("kor", "apple", "red");
        sut.addSurveyResult("kor", "apple", "blue");

        int actual = sut.getMatchedCount("kor", "apple", "red");

        assertEquals(1, actual);
    }

    @ParameterizedTest
    @MethodSource("provideSurveyQuery")
    @DisplayName("여러 설문조사 결과 중 매칭되는 건의 개수를 반환한다.")
    void surveyReturnsMatchedCountGivenMultiSurveyResults(String subject, String fruit, String color, int expected) {
        Survey sut = new Survey();
        sut.addSurveyResult("kor", "apple", "red");
        sut.addSurveyResult("kor", "pear", "blue");
        sut.addSurveyResult("eng", "apple", "red");
        sut.addSurveyResult("eng", "orange", "blue");
        sut.addSurveyResult("math", "apple", "green");

        int actual = sut.getMatchedCount(subject, fruit, color);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSurveyQuery() {
        return Stream.of(
                Arguments.of("kor", "apple", "red", 1),
                Arguments.of("kor", "-", "blue", 1),
                Arguments.of("eng", "-", "-", 2),
                Arguments.of("-", "-", "red", 2),
                Arguments.of("-", "-", "-", 5)
        );
    }
}

class Survey {

    private final Map<SurveyKey, Integer> results = new HashMap<>();

    public void addSurveyResult(String subject, String fruit, String color) {
        SurveyKeys surveyKeys = new SurveyKeys(new SurveyKey(subject, fruit, color));
        for (SurveyKey surveyKey : surveyKeys.getSurveyKeys()) {
            results.merge(surveyKey, 1, Integer::sum);
        }
    }

    public int getMatchedCount(String subject, String fruit, String color) {
        return results.getOrDefault(new SurveyKey(subject, fruit, color), 0);
    }
}

class SurveyKeys {

    private static final String WILDCARD = "-";

    private final SurveyKey surveyKey;

    public SurveyKeys(SurveyKey surveyKey) {
        this.surveyKey = surveyKey;
    }

    public List<SurveyKey> getSurveyKeys() {
        return List.of(
                new SurveyKey(WILDCARD, WILDCARD, WILDCARD),
                new SurveyKey(WILDCARD, WILDCARD, surveyKey.getColor()),
                new SurveyKey(WILDCARD, surveyKey.getFruit(), WILDCARD),
                new SurveyKey(surveyKey.getSubject(), WILDCARD, WILDCARD),
                new SurveyKey(WILDCARD, surveyKey.getFruit(), surveyKey.getColor()),
                new SurveyKey(surveyKey.getSubject(), WILDCARD, surveyKey.getColor()),
                new SurveyKey(surveyKey.getSubject(), surveyKey.getFruit(), WILDCARD),
                new SurveyKey(surveyKey.getSubject(), surveyKey.getFruit(), surveyKey.getColor())
        );
    }
}

class SurveyKey {

    private final String subject;
    private final String fruit;
    private final String color;

    public SurveyKey(String subject, String fruit, String color) {
        this.subject = subject;
        this.fruit = fruit;
        this.color = color;
    }

    public String getSubject() {
        return subject;
    }

    public String getFruit() {
        return fruit;
    }

    public String getColor() {
        return color;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        SurveyKey surveyKey = (SurveyKey) o;
        return Objects.equals(subject, surveyKey.subject) && Objects.equals(fruit, surveyKey.fruit) && Objects.equals(color, surveyKey.color);
    }

    @Override
    public int hashCode() {
        return Objects.hash(subject, fruit, color);
    }
}
~~~
