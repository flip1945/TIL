# 접미사 배열 (Silver 4)

### 문제 설명

접미사 배열은 문자열 S의 모든 접미사를 사전순으로 정렬해 놓은 배열이다.

baekjoon의 접미사는 baekjoon, aekjoon, ekjoon, kjoon, joon, oon, on, n 으로 총 8가지가 있고, 이를 사전순으로 정렬하면, aekjoon, baekjoon, ekjoon, joon, kjoon, n, on, oon이 된다.

문자열 S가 주어졌을 때, 모든 접미사를 사전순으로 정렬한 다음 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/11656

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String word = scanner.nextLine();

        getSortedSuffix(word).forEach(System.out::println);
    }

    static List<String> getSortedSuffix(String word) {
        return getSuffix(word).stream()
                .sorted()
                .collect(Collectors.toList());
    }

    static List<String> getSuffix(String word) {
        return IntStream.range(0, word.length())
                .mapToObj(word::substring)
                .collect(Collectors.toList());
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getSuffixTest() {
        assertEquals(List.of("abc", "bc", "c"), getSuffix("abc"));
        assertEquals(List.of("bc", "c"), getSuffix("bc"));
        assertEquals(List.of("dcba", "cba", "ba", "a"), getSuffix("dcba"));
        assertEquals(List.of("aaaa", "aaa", "aa", "a"), getSuffix("aaaa"));
        assertEquals(List.of("baekjoon", "aekjoon", "ekjoon", "kjoon", "joon", "oon", "on", "n"), getSuffix("baekjoon"));
    }

    @Test
    void getSortedSuffixTest() {
        assertEquals(List.of("abc", "bc", "c"), getSortedSuffix("abc"));
        assertEquals(List.of("a", "ba", "cba", "dcba"), getSortedSuffix("dcba"));
        assertEquals(List.of("a"), getSortedSuffix("a"));
        assertEquals(List.of("aekjoon", "baekjoon", "ekjoon", "joon", "kjoon", "n", "on", "oon"), getSortedSuffix("baekjoon"));
    }

    private List<String> getSortedSuffix(String word) {
        return getSuffix(word).stream()
                .sorted()
                .collect(Collectors.toList());
    }

    private List<String> getSuffix(String word) {
        return IntStream.range(0, word.length())
                .mapToObj(word::substring)
                .collect(Collectors.toList());
    }
}
~~~
