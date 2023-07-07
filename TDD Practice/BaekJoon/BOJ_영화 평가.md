# 영화 평가 (Bronze 1)

### 문제 설명

스타트링크에는 영화 감상 동아리가 있다. 영화 동아리에는 총 N명의 구성원이 있고, 매주 모여서 영화 한 편을 본다. 영화를 본 뒤, 각 사람은 0보다 크거나 같고, 100보다 작거나 같은 정수로 영화를 평가한다. 모든 구성원이 영화 평가를 마치면 동아리장은 최종 점수를 계산한다.

최종 평점은 가장 낮은 평가 L개와 가장 높은 평가 H개를 뺀 나머지 점수의 평균이다.

영화 감상 동아리의 각 회원이 남긴 평가가 입력으로 주어졌을 때, 최종 점수를 계산해보자.

출처 : https://www.acmicpc.net/problem/23278

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int l = scanner.nextInt();
        int h = scanner.nextInt();
        List<Integer> ratings = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextInt())
                .collect(Collectors.toList());

        MovieReview movieReview = new MovieReview(n, l, h, ratings);
        System.out.println(movieReview.getAvgRating());
    }
}

class MovieReview {

    private final int countOfReviewers;
    private final int lowThreshold;
    private final int highThreshold;
    private final List<Integer> ratings;

    public MovieReview(int countOfReviewers, int lowThreshold, int highThreshold, List<Integer> ratings) {
        this.countOfReviewers = countOfReviewers;
        this.lowThreshold = lowThreshold;
        this.highThreshold = highThreshold;
        this.ratings = ratings;
    }

    public double getAvgRating() {
        return sumOfRatingsOfValidReview() / getCountOfValidReviewers();
    }

    private double sumOfRatingsOfValidReview() {
        return IntStream.range(lowThreshold, countOfReviewers - highThreshold)
                .mapToDouble(getSortedRatings()::get)
                .sum();
    }

    private List<Integer> getSortedRatings() {
        return new ArrayList<>(ratings).stream()
                .sorted()
                .collect(Collectors.toList());
    }

    private int getCountOfValidReviewers() {
        return countOfReviewers - lowThreshold - highThreshold;
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
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideFractions")
    @DisplayName("주어진 분수에 대해 맞는 대분수를 문자열로 반환한다.")
    void getMixedFractionTest(int countOfReviewers, int lowThreshold, int highThreshold, List<Integer> ratings, double expected) {
        MovieReview sut = new MovieReview(countOfReviewers, lowThreshold, highThreshold, ratings);

        double actual = sut.getAvgRating();

        assertEquals(expected, actual, 0.00000000001);
    }

    private static Stream<Arguments> provideFractions() {
        return Stream.of(
                Arguments.of(5, 0, 0, List.of(70, 99, 96, 0, 30), 59.0),
                Arguments.of(3, 1, 1, List.of(91, 90, 50), 90.0),
                Arguments.of(8, 2, 3, List.of(23, 23, 23, 23, 23, 23, 23, 23), 23),
                Arguments.of(10, 1, 4, List.of(31, 52, 20, 86, 47, 76, 82, 27, 42, 29), 35.2),
                Arguments.of(10, 2, 2, List.of(1, 1, 0, 0, 1, 1, 0, 1, 0, 2), 0.6666666666666666)
        );
    }
}

class MovieReview {

    private final int countOfReviewers;
    private final int lowThreshold;
    private final int highThreshold;
    private final List<Integer> ratings;

    public MovieReview(int countOfReviewers, int lowThreshold, int highThreshold, List<Integer> ratings) {
        this.countOfReviewers = countOfReviewers;
        this.lowThreshold = lowThreshold;
        this.highThreshold = highThreshold;
        this.ratings = ratings;
    }

    public double getAvgRating() {
        return sumOfRatingsOfValidReview() / getCountOfValidReviewers();
    }

    private double sumOfRatingsOfValidReview() {
        return IntStream.range(lowThreshold, countOfReviewers - highThreshold)
                .mapToDouble(getSortedRatings()::get)
                .sum();
    }

    private List<Integer> getSortedRatings() {
        return new ArrayList<>(ratings).stream()
                .sorted()
                .collect(Collectors.toList());
    }

    private int getCountOfValidReviewers() {
        return countOfReviewers - lowThreshold - highThreshold;
    }
}
~~~
