# DNA (Silver 5)

### 문제 설명

DNA란 어떤 유전물질을 구성하는 분자이다. 이 DNA는 서로 다른 4가지의 뉴클레오티드로 이루어져 있다(Adenine, Thymine, Guanine, Cytosine). 우리는 어떤 DNA의 물질을 표현할 때, 이 DNA를 이루는 뉴클레오티드의 첫글자를 따서 표현한다. 만약에 Thymine-Adenine-Adenine-Cytosine-Thymine-Guanine-Cytosine-Cytosine-Guanine-Adenine-Thymine로 이루어진 DNA가 있다고 하면, “TAACTGCCGAT”로 표현할 수 있다. 그리고 Hamming Distance란 길이가 같은 두 DNA가 있을 때, 각 위치의 뉴클오티드 문자가 다른 것의 개수이다. 만약에 “AGCAT"와 ”GGAAT"는 첫 번째 글자와 세 번째 글자가 다르므로 Hamming Distance는 2이다.

우리가 할 일은 다음과 같다. N개의 길이 M인 DNA s1, s2, ..., sn가 주어져 있을 때 Hamming Distance의 합이 가장 작은 DNA s를 구하는 것이다. 즉, s와 s1의 Hamming Distance + s와 s2의 Hamming Distance + s와 s3의 Hamming Distance ... 의 합이 최소가 된다는 의미이다.

출처 : https://www.acmicpc.net/problem/1969

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(bf.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        List<String> dna = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            dna.add(bf.readLine());
        }

        String minHammingDistanceDna = findMinHammingDistanceDna(dna, m);
        System.out.println(minHammingDistanceDna);
        System.out.println(getTotalHammingDistance(minHammingDistanceDna, dna));
    }

    private static String findMinHammingDistanceDna(List<String> dna, int danSize) {
        StringBuilder minHammingDistanceDna = new StringBuilder();
        for (int i = 0; i < danSize; i++) {
            int[] alphabet = new int[26];
            for (String d : dna) {
                alphabet[uppercaseToIndex(d.charAt(i))]++;
            }

            minHammingDistanceDna.append(getMostFrequentLetter(alphabet));
        }
        return minHammingDistanceDna.toString();
    }

    private static int uppercaseToIndex(char character) {
        return character - 65;
    }

    private static char getMostFrequentLetter(int[] alphabet) {
        int maxIndex = 0;
        int index = 0;
        for (int j = 25; j >= 0; j--) {
            if (alphabet[j] >= maxIndex) {
                maxIndex = alphabet[j];
                index = j;
            }
        }
        return indexToUppercase(index);
    }

    private static char indexToUppercase(int index) {
        return (char)(index + 65);
    }

    private static int getTotalHammingDistance(String originalDna, List<String> dna) {
        return dna.stream().mapToInt(d -> calculateHammingDistance(originalDna, d)).sum();
    }

    private static int calculateHammingDistance(String originalDna, String comparedDna) {
        int hammingDistance = 0;
        for (int i = 0; i < originalDna.length(); i++) {
            if (originalDna.charAt(i) != comparedDna.charAt(i)) {
                hammingDistance++;
            }
        }
        return hammingDistance;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void calculateHammingDistanceTest() {
        String dna1 = "TATGATAC";
        String dna2 = "TAAGCTAC";
        String dna3 = "AAAGATCC";

        assertEquals(2, calculateHammingDistance(dna1, dna2));
        assertEquals(3, calculateHammingDistance(dna1, dna3));
        assertEquals(3, calculateHammingDistance(dna2, dna3));
    }

    @Test
    void getTotalHammingDistanceTest() {
        List<String> dna = List.of("TATGATAC", "TAAGCTAC", "AAAGATCC", "TGAGATAC", "TAAGATGT");

        assertEquals(40, getTotalHammingDistance("ZZZZZZZZ", dna));
        assertEquals(7, getTotalHammingDistance("TAAGATAC", dna));
        assertEquals(10, getTotalHammingDistance("TATGATAC", dna));
    }

    @Test
    void findMinHammingDistanceDnaTest() {
        List<String> dna1 = List.of("TATGATAC", "TAAGCTAC", "AAAGATCC", "TGAGATAC", "TAAGATGT");
        List<String> dna2 = List.of("ACGTACGTAC", "CCGTACGTAG", "GCGTACGTAT", "TCGTACGTAA");
        List<String> dna3 = List.of("AAAA", "BBBB", "CCCC");
        List<String> dna4 = List.of("A", "Z");

        assertEquals("TAAGATAC", findMinHammingDistanceDna(dna1, 8));
        assertEquals("ACGTACGTAA", findMinHammingDistanceDna(dna2, 10));
        assertEquals("AAAA", findMinHammingDistanceDna(dna3, 4));
        assertEquals("A", findMinHammingDistanceDna(dna4, 1));
    }

    private String findMinHammingDistanceDna(List<String> dna, int danSize) {
        StringBuilder minHammingDistanceDna = new StringBuilder();
        for (int i = 0; i < danSize; i++) {
            int[] alphabet = new int[26];
            for (String d : dna) {
                alphabet[uppercaseToIndex(d.charAt(i))]++;
            }

            minHammingDistanceDna.append(getMostFrequentLetter(alphabet));
        }
        return minHammingDistanceDna.toString();
    }

    private int uppercaseToIndex(char character) {
        return character - 65;
    }

    private char getMostFrequentLetter(int[] alphabet) {
        int maxIndex = 0;
        int index = 0;
        for (int j = 25; j >= 0; j--) {
            if (alphabet[j] >= maxIndex) {
                maxIndex = alphabet[j];
                index = j;
            }
        }
        return indexToUppercase(index);
    }

    private char indexToUppercase(int index) {
        return (char)(index + 65);
    }

    private int getTotalHammingDistance(String originalDna, List<String> dna) {
        return dna.stream().mapToInt(d -> calculateHammingDistance(originalDna, d)).sum();
    }

    private int calculateHammingDistance(String originalDna, String comparedDna) {
        int hammingDistance = 0;
        for (int i = 0; i < originalDna.length(); i++) {
            if (originalDna.charAt(i) != comparedDna.charAt(i)) {
                hammingDistance++;
            }
        }
        return hammingDistance;
    }
}
~~~
