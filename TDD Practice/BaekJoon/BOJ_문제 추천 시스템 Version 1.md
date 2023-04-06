# 문제 추천 시스템 Version 1 (Gold 4)

### 문제 설명

tony9402는 최근 깃헙에 코딩테스트 대비 문제를 직접 뽑아서 "문제 번호, 난이도"로 정리해놨다.

깃헙을 이용하여 공부하시는 분들을 위해 새로운 기능을 추가해보려고 한다.

만들려고 하는 명령어는 총 3가지가 있다. 아래 표는 각 명령어에 대한 설명이다.

**recommend** $x$ 	
~~~
x가 1인 경우 추천 문제 리스트에서 가장 어려운 문제의 번호를 출력한다.

만약 가장 어려운 문제가 여러 개라면 문제 번호가 큰 것으로 출력한다.

x가 -1인 경우 추천 문제 리스트에서 가장 쉬운 문제의 번호를 출력한다.

만약 가장 쉬운 문제가 여러 개라면 문제 번호가 작은 것으로 출력한다.
~~~

**add** $P$ $L$ 	
~~~
추천 문제 리스트에 난이도가 L인 문제 번호 P를 추가한다. 

(추천 문제 리스트에 없는 문제 번호 P만 입력으로 주어진다. 

이전에 추천 문제 리스트에 있던 문제 번호가 다른 난이도로 다시 들어 올 수 있다.)
~~~

**solved** $P$
~~~
추천 문제 리스트에서 문제 번호 P를 제거한다. (추천 문제 리스트에 있는 문제 번호 P만 입력으로 주어진다.)
~~~

명령어 **recommend**는 추천 문제 리스트에 문제가 하나 이상 있을 때만 주어진다.

명령어 **solved**는 추천 문제 리스트에 문제 번호가 하나 이상 있을 때만 주어진다.

위 명령어들을 수행하는 추천 시스템을 만들어보자.

출처 : https://www.acmicpc.net/problem/21939

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(bufferedReader.readLine());
        List<Problem> problems = new ArrayList<>(n + 1);
        StringTokenizer st;
        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(bufferedReader.readLine());
            problems.add(new Problem(Integer.parseInt(st.nextToken()), Integer.parseInt(st.nextToken())));
        }

        ProblemRecommender recommender = new ProblemRecommender(problems);

        int m = Integer.parseInt(bufferedReader.readLine());
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(bufferedReader.readLine());
            String command = st.nextToken();

            switch (command) {
                case "recommend":
                    result.append(recommender.recommend(Integer.parseInt(st.nextToken())));
                    result.append("\n");
                    break;
                case "add":
                    recommender.add(Integer.parseInt(st.nextToken()), Integer.parseInt(st.nextToken()));
                    break;
                case "solved":
                    recommender.solved(Integer.parseInt(st.nextToken()));
                    break;
            }
        }

        System.out.print(result);
    }
}

class ProblemRecommender {
    private final Map<Integer, Integer> recommendedProblems = new HashMap<>();
    private final PriorityQueue<Problem> maxQueue;
    private final PriorityQueue<Problem> minQueue;

    public ProblemRecommender(List<Problem> problems) {
        initProblems(problems);
        this.maxQueue = initMaxQueue(problems);
        this.minQueue = initMinQueue(problems);
    }

    private void initProblems(List<Problem> problems) {
        for (Problem problem : problems) {
            recommendedProblems.put(problem.getNumber(), problem.getDifficulty());
        }
    }

    private PriorityQueue<Problem> initMaxQueue(List<Problem> problems) {
        PriorityQueue<Problem> queue = new PriorityQueue<>((p1, p2) -> (p2.getDifficulty() - p1.getDifficulty() != 0) ?
                p2.getDifficulty() - p1.getDifficulty() : p2.getNumber() - p1.getNumber());
        queue.addAll(problems);
        return queue;
    }

    private PriorityQueue<Problem> initMinQueue(List<Problem> problems) {
        PriorityQueue<Problem> queue = new PriorityQueue<>(Comparator.comparingInt(Problem::getDifficulty)
                .thenComparing(Problem::getNumber));
        queue.addAll(problems);
        return queue;
    }

    public int recommend(int x) {
        if (x == 1) {
            return getRecommendProblem(maxQueue);
        }
        return getRecommendProblem(minQueue);
    }

    private int getRecommendProblem(PriorityQueue<Problem> queue) {
        Problem problem = queue.poll();
        while (!recommendedProblems.containsKey(problem.getNumber()) || problem.getDifficulty() != recommendedProblems.get(problem.getNumber())) {
            problem = queue.poll();
        }
        queue.add(problem);
        return problem.getNumber();
    }

    public void add(int problemNumber, int difficulty) {
        recommendedProblems.put(problemNumber, difficulty);
        maxQueue.add(new Problem(problemNumber, difficulty));
        minQueue.add(new Problem(problemNumber, difficulty));
    }

    public void solved(int problemNumber) {
        recommendedProblems.remove(problemNumber);
    }
}

class Problem {
    private final int number;
    private final int difficulty;

    public Problem(int number, int difficulty) {
        this.number = number;
        this.difficulty = difficulty;
    }

    public int getNumber() {
        return number;
    }

    public int getDifficulty() {
        return difficulty;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        return number == ((Problem) o).number;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import java.util.*;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    private List<Problem> problems;

    @BeforeEach
    void setUp() {
        problems = new ArrayList<>();
        problems.add(new Problem(1000, 1));
        problems.add(new Problem(1001, 2));
        problems.add(new Problem(19998, 78));
        problems.add(new Problem(2667, 37));
        problems.add(new Problem(2042, 55));
    }

    @AfterEach
    void tearDown() {
        problems.clear();
    }

    @Test
    void recommendTest() {
        ProblemRecommender recommender = new ProblemRecommender(problems);

        assertEquals(19998, recommender.recommend(1));
        assertEquals(1000, recommender.recommend(-1));
    }

    @Test
    void addTest() {
        ProblemRecommender recommender = new ProblemRecommender(problems);

        recommender.add(10000, 100);

        assertEquals(10000, recommender.recommend(1));
        assertEquals(1000, recommender.recommend(-1));
    }

    @Test
    void solvedTest() {
        ProblemRecommender recommender = new ProblemRecommender(problems);

        recommender.solved(19998);
        recommender.solved(1000);

        assertEquals(2042, recommender.recommend(1));
        assertEquals(1001, recommender.recommend(-1));
    }

    @Test
    void problemRecommenderTest() {
        ProblemRecommender recommender = new ProblemRecommender(problems);

        recommender.add(1402, 59);

        assertEquals(19998, recommender.recommend(1));

        recommender.solved(1000);
        recommender.solved(19998);

        assertEquals(1402, recommender.recommend(1));
        assertEquals(1001, recommender.recommend(-1));

        recommender.solved(1001);

        assertEquals(2667, recommender.recommend(-1));
    }

    @Test
    void fastSolveAddTest() {
        List<Problem> problems = new ArrayList<>();
        problems.add(new Problem(1000, 1));
        problems.add(new Problem(2000, 1));
        ProblemRecommender recommender = new ProblemRecommender(problems);

        assertEquals(1000, recommender.recommend(-1));

        recommender.solved(1000);
        recommender.add(1000, 1000);

        assertEquals(2000, recommender.recommend(-1));
    }
}

class ProblemRecommender {
    private final Map<Integer, Integer> recommendedProblems = new HashMap<>();
    private final PriorityQueue<Problem> maxQueue;
    private final PriorityQueue<Problem> minQueue;

    public ProblemRecommender(List<Problem> problems) {
        initProblems(problems);
        this.maxQueue = initMaxQueue(problems);
        this.minQueue = initMinQueue(problems);
    }

    private void initProblems(List<Problem> problems) {
        for (Problem problem : problems) {
            recommendedProblems.put(problem.getNumber(), problem.getDifficulty());
        }
    }

    private PriorityQueue<Problem> initMaxQueue(List<Problem> problems) {
        PriorityQueue<Problem> queue = new PriorityQueue<>((p1, p2) -> (p2.getDifficulty() - p1.getDifficulty() != 0) ?
                p2.getDifficulty() - p1.getDifficulty() : p2.getNumber() - p1.getNumber());
        queue.addAll(problems);
        return queue;
    }

    private PriorityQueue<Problem> initMinQueue(List<Problem> problems) {
        PriorityQueue<Problem> queue = new PriorityQueue<>(Comparator.comparingInt(Problem::getDifficulty)
                .thenComparing(Problem::getNumber));
        queue.addAll(problems);
        return queue;
    }

    public int recommend(int x) {
        if (x == 1) {
            return getRecommendProblem(maxQueue);
        }
        return getRecommendProblem(minQueue);
    }

    private int getRecommendProblem(PriorityQueue<Problem> queue) {
        Problem problem = queue.poll();
        while (!recommendedProblems.containsKey(problem.getNumber()) || problem.getDifficulty() != recommendedProblems.get(problem.getNumber())) {
            problem = queue.poll();
        }
        queue.add(problem);
        return problem.getNumber();
    }

    public void add(int problemNumber, int difficulty) {
        recommendedProblems.put(problemNumber, difficulty);
        maxQueue.add(new Problem(problemNumber, difficulty));
        minQueue.add(new Problem(problemNumber, difficulty));
    }

    public void solved(int problemNumber) {
        recommendedProblems.remove(problemNumber);
    }
}

class Problem {
    private final int number;
    private final int difficulty;

    public Problem(int number, int difficulty) {
        this.number = number;
        this.difficulty = difficulty;
    }

    public int getNumber() {
        return number;
    }

    public int getDifficulty() {
        return difficulty;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        return number == ((Problem) o).number;
    }
}
~~~
