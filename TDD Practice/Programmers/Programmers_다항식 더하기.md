# 다항식 더하기 (Level 0)

### 문제 설명

한 개 이상의 항의 합으로 이루어진 식을 다항식이라고 합니다. 다항식을 계산할 때는 동류항끼리 계산해 정리합니다. 덧셈으로 이루어진 다항식 polynomial이 매개변수로 주어질 때, 동류항끼리 더한 결괏값을 문자열로 return 하도록 solution 함수를 완성해보세요. 같은 식이라면 가장 짧은 수식을 return 합니다.

#### 제한사항

* 0 < polynomial에 있는 수 < 100
* polynomial에 변수는 'x'만 존재합니다.
* polynomial은 0부터 9까지의 정수, 공백, ‘x’, ‘+'로 이루어져 있습니다.
* 항과 연산기호 사이에는 항상 공백이 존재합니다.
* 공백은 연속되지 않으며 시작이나 끝에는 공백이 없습니다.
* 하나의 항에서 변수가 숫자 앞에 오는 경우는 없습니다.
* " + 3xx + + x7 + "와 같은 잘못된 입력은 주어지지 않습니다.
* "012x + 001"처럼 0을 제외하고는 0으로 시작하는 수는 없습니다.
* 문자와 숫자 사이의 곱하기는 생략합니다.
* polynomial에는 일차 항과 상수항만 존재합니다.
* 계수 1은 생략합니다.
* 결괏값에 상수항은 마지막에 둡니다.
* 0 < polynomial의 길이 < 50

출처 : https://programmers.co.kr/learn/courses/30/lessons/120863

---

#### 풀이

~~~java
class Solution {
    public String solution(String polynomial) {
        int x = getXTerm(polynomial);
        int constant = getConstantTerm(polynomial);

        return getXTermString(x) + getConstantTermString(x, constant);
    }

    private String getXTermString(int x) {
        if (x == 0) {
            return "";
        }
        return (x == 1) ? "x" : String.format("%dx", x);
    }

    private String getConstantTermString(int x, int constant) {
        if (constant == 0) {
            return "";
        }
        return (x != 0) ? String.format(" + %d", constant): String.format("%d", constant);
    }

    private int getXTerm(String polynomial) {
        int x = 0;
        for (String term : polynomial.split(" \\+ ")) {
            if (term.endsWith("x")) {
                x += parseXTerm(term);
            }
        }
        return x;
    }

    private int parseXTerm(String term) {
        term = term.substring(0, term.length() - 1);
        if ("".equals(term)) {
            return 1;
        }
        return Integer.parseInt(term);
    }

    private int getConstantTerm(String polynomial) {
        int constant = 0;
        for (String term : polynomial.split(" \\+ ")) {
            if (!term.endsWith("x")) {
                constant += Integer.parseInt(term);
            }
        }
        return constant;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getXTermTest() {
        String polynomial1 = "3x + 7";
        String polynomial2 = "x";
        String polynomial3 = "x + x";
        String polynomial4 = "x + x + x";
        String polynomial5 = "3x + 8x";

        assertEquals(3, getXTerm(polynomial1));
        assertEquals(1, getXTerm(polynomial2));
        assertEquals(2, getXTerm(polynomial3));
        assertEquals(3, getXTerm(polynomial4));
        assertEquals(11, getXTerm(polynomial5));
    }

    @Test
    void getConstantTermTest() {
        String polynomial1 = "3x + 7";
        String polynomial2 = "3x";
        String polynomial3 = "x + x + x";
        String polynomial4 = "3 + 10";

        assertEquals(7, getConstantTerm(polynomial1));
        assertEquals(0, getConstantTerm(polynomial2));
        assertEquals(0, getConstantTerm(polynomial3));
        assertEquals(13, getConstantTerm(polynomial4));
    }

    @Test
    void solutionTest() {
        String polynomial1 = "3x + 7 + x";
        String polynomial2 = "x + x + x";
        String polynomial3 = "x";
        String polynomial4 = "3 + 7";

        assertEquals("4x + 7", solution(polynomial1));
        assertEquals("3x", solution(polynomial2));
        assertEquals("x", solution(polynomial3));
        assertEquals("10", solution(polynomial4));
    }

    public String solution(String polynomial) {
        int x = getXTerm(polynomial);
        int constant = getConstantTerm(polynomial);

        return getXTermString(x) + getConstantTermString(x, constant);
    }

    private String getXTermString(int x) {
        if (x == 0) {
            return "";
        }
        return (x == 1) ? "x" : String.format("%dx", x);
    }

    private String getConstantTermString(int x, int constant) {
        if (constant == 0) {
            return "";
        }
        return (x != 0) ? String.format(" + %d", constant): String.format("%d", constant);
    }

    private int getXTerm(String polynomial) {
        int x = 0;
        for (String term : polynomial.split(" \\+ ")) {
            if (term.endsWith("x")) {
                x += parseXTerm(term);
            }
        }
        return x;
    }

    private int parseXTerm(String term) {
        term = term.substring(0, term.length() - 1);
        if ("".equals(term)) {
            return 1;
        }
        return Integer.parseInt(term);
    }

    private int getConstantTerm(String polynomial) {
        int constant = 0;
        for (String term : polynomial.split(" \\+ ")) {
            if (!term.endsWith("x")) {
                constant += Integer.parseInt(term);
            }
        }
        return constant;
    }
}
~~~
