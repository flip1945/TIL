# Java vs C++ (Silver 3)

### 문제 설명

Java 예찬론자 김동규와 C++ 옹호가 김동혁은 서로 어떤 프로그래밍 언어가 최고인지 몇 시간동안 토론을 하곤 했다. 동규는 Java가 명확하고 에러가 적은 프로그램을 만든다고 주장했고, 동혁이는 Java는 프로그램이 느리고, 긴 소스 코드를 갖는 점과 제네릭 배열의 인스턴스화의 무능력을 비웃었다.

또, 김동규와 김동혁은 변수 이름을 짓는 방식도 서로 달랐다. Java에서는 변수의 이름이 여러 단어로 이루어져있을 때, 다음과 같은 방법으로 변수명을 짓는다. 

첫 단어는 소문자로 쓰고, 다음 단어부터는 첫 문자만 대문자로 쓴다. 또, 모든 단어는 붙여쓴다. 따라서 Java의 변수명은 javaIdentifier, longAndMnemonicIdentifier, name, bAEKJOON과 같은 형태이다.

반면에 C++에서는 변수명에 소문자만 사용한다. 단어와 단어를 구분하기 위해서 밑줄('_')을 이용한다. C++ 변수명은 c_identifier, long_and_mnemonic_identifier, name, b_a_e_k_j_o_o_n과 같은 형태이다.

이 둘의 싸움을 부질없다고 느낀 재원이는 C++형식의 변수명을 Java형식의 변수명으로, 또는 그 반대로 바꿔주는 프로그램을 만들려고 한다. 각 언어의 변수명 형식의 위의 설명을 따라야 한다.

재원이의 프로그램은 가장 먼저 변수명을 입력으로 받은 뒤, 이 변수명이 어떤 언어 형식인지를 알아내야 한다. 그 다음, C++형식이라면 Java형식으로, Java형식이라면 C++형식으로 바꾸면 된다. 만약 C++형식과 Java형식 둘 다 아니라면, 에러를 발생시킨다. 변수명을 변환할 때, 단어의 순서는 유지되어야 한다.

재원이는 프로그램을 만들려고 했으나, 너무 귀찮은 나머지 이를 문제를 읽는 사람의 몫으로 맡겨놨다.

재원이가 만들려고 한 프로그램을 대신 만들어보자.

출처 : https://www.acmicpc.net/problem/3613

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        VariableConverter converter = new VariableConverter();
        System.out.println(converter.convertJavaOrCppVariable(scanner.nextLine()));
    }
}

class VariableConverter {

    private static final String ERROR_WORD = "Error!";
    private static final String ERROR_CPP_SEPARATOR = "__";
    private static final String CPP_SEPARATOR = "_";

    public String convertJavaOrCppVariable(String variable) {
        if (isErrorVariable(variable)) {
            return ERROR_WORD;
        } else if (isJavaVariable(variable)) {
            return convertToCppVariable(variable);
        } else if (isCppVariable(variable)) {
            return convertToJavaVariable(variable);
        }
        return variable;
    }

    private boolean isErrorVariable(String variable) {
        return Character.isUpperCase(variable.charAt(0))
                || (variable.contains(CPP_SEPARATOR) && variable.chars().anyMatch(Character::isUpperCase))
                || variable.contains(ERROR_CPP_SEPARATOR)
                || String.valueOf(variable.charAt(0)).equals(CPP_SEPARATOR)
                || String.valueOf(variable.charAt(variable.length() - 1)).equals(CPP_SEPARATOR);
    }

    private boolean isJavaVariable(String variable) {
        return variable.chars().anyMatch(Character::isUpperCase) && !variable.contains(CPP_SEPARATOR);
    }

    private String convertToCppVariable(String variable) {
        StringBuilder result = new StringBuilder();
        for (char letter : variable.toCharArray()) {
            result.append(convertToCppLetter(letter));
        }
        return result.toString();
    }

    private String convertToCppLetter(char letter) {
        if (Character.isUpperCase(letter)) {
            return CPP_SEPARATOR + Character.toLowerCase(letter);
        }
        return String.valueOf(letter);
    }

    private boolean isCppVariable(String variable) {
        return variable.contains(CPP_SEPARATOR) && variable.chars().noneMatch(Character::isUpperCase);
    }

    private String convertToJavaVariable(String variable) {
        StringBuilder result = new StringBuilder();
        for (String word : variable.split(CPP_SEPARATOR)) {
            result.append(replaceFirstLetterToUppercase(word));
        }
        return replaceFirstLetterToLowercase(result.toString());
    }

    private String replaceFirstLetterToUppercase(String word) {
        return word.substring(0, 1).toUpperCase() + word.substring(1);
    }

    private String replaceFirstLetterToLowercase(String word) {
        return word.substring(0, 1).toLowerCase() + word.substring(1);
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

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideVariables")
    @DisplayName("Java 변수는 c++ 변수로 c++ 변수는 Java 변수로 변환한다.")
    void convertJavaOrCppVariableTest(String variable, String expected) {
        VariableConverter sut = new VariableConverter();

        String actual = sut.convertJavaOrCppVariable(variable);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideVariables() {
        return Stream.of(
                Arguments.of("aVariable", "a_variable")
                , Arguments.of("javaIdentifier", "java_identifier")
                , Arguments.of("longAndMnemonicIdentifier", "long_and_mnemonic_identifier")
                , Arguments.of("bAEKJOON", "b_a_e_k_j_o_o_n")
                , Arguments.of("a_variable", "aVariable")
                , Arguments.of("c_identifier", "cIdentifier")
                , Arguments.of("long_and_mnemonic_identifier", "longAndMnemonicIdentifier")
                , Arguments.of("b_a_e_k_j_o_o_n", "bAEKJOON")
                , Arguments.of("name", "name")
                , Arguments.of("a", "a")
                , Arguments.of("b", "b")
                , Arguments.of("aJava_cpp", "Error!")
                , Arguments.of("Aaa", "Error!")
                , Arguments.of("bbb__aaa_d", "Error!")
                , Arguments.of("bbb___aaa_d", "Error!")
                , Arguments.of("_aaa", "Error!")
                , Arguments.of("aaa_", "Error!")
        );
    }
}

class VariableConverter {

    private static final String ERROR_WORD = "Error!";
    private static final String ERROR_CPP_SEPARATOR = "__";
    private static final String CPP_SEPARATOR = "_";

    public String convertJavaOrCppVariable(String variable) {
        if (isErrorVariable(variable)) {
            return ERROR_WORD;
        } else if (isJavaVariable(variable)) {
            return convertToCppVariable(variable);
        } else if (isCppVariable(variable)) {
            return convertToJavaVariable(variable);
        }
        return variable;
    }

    private boolean isErrorVariable(String variable) {
        return Character.isUpperCase(variable.charAt(0))
                || (variable.contains(CPP_SEPARATOR) && variable.chars().anyMatch(Character::isUpperCase))
                || variable.contains(ERROR_CPP_SEPARATOR)
                || String.valueOf(variable.charAt(0)).equals(CPP_SEPARATOR)
                || String.valueOf(variable.charAt(variable.length() - 1)).equals(CPP_SEPARATOR);
    }

    private boolean isJavaVariable(String variable) {
        return variable.chars().anyMatch(Character::isUpperCase) && !variable.contains(CPP_SEPARATOR);
    }

    private String convertToCppVariable(String variable) {
        StringBuilder result = new StringBuilder();
        for (char letter : variable.toCharArray()) {
            result.append(convertToCppLetter(letter));
        }
        return result.toString();
    }

    private String convertToCppLetter(char letter) {
        if (Character.isUpperCase(letter)) {
            return CPP_SEPARATOR + Character.toLowerCase(letter);
        }
        return String.valueOf(letter);
    }

    private boolean isCppVariable(String variable) {
        return variable.contains(CPP_SEPARATOR) && variable.chars().noneMatch(Character::isUpperCase);
    }

    private String convertToJavaVariable(String variable) {
        StringBuilder result = new StringBuilder();
        for (String word : variable.split(CPP_SEPARATOR)) {
            result.append(replaceFirstLetterToUppercase(word));
        }
        return replaceFirstLetterToLowercase(result.toString());
    }

    private String replaceFirstLetterToUppercase(String word) {
        return word.substring(0, 1).toUpperCase() + word.substring(1);
    }

    private String replaceFirstLetterToLowercase(String word) {
        return word.substring(0, 1).toLowerCase() + word.substring(1);
    }
}
~~~
