# 카이사르 암호 (Bronze 2)

### 문제 설명

가이우스 율리우스 카이사르(Gaius Julius Caesar)는 고대 로마 군인이자 정치가였다. 카이사르는 비밀스럽게 편지를 쓸 때, 'A'를 'D로', 'B'를 'E'로, 'C'를 'F'로... 이런 식으로 알파벳 문자를 3개씩 건너뛰어 적었다고 한다.

26개의 대문자 알파벳으로 이루어진 단어를 카이사르 암호 형식으로 3문자를 옮겨 겹치지 않게 나열하여 얻은 카이사르 단어가 있다. 이 카이사르 단어를 원래 단어로 돌려놓는 프로그램을 작성하시오.

각 문자별로 변환 전과 변환 후를 나타낸 건 아래와 같다.

~~~
변환전    A B C D E F G H I J K L M N O P Q R S T U V W X Y Z 
변환후    D E F G H I J K L M N O P Q R S T U V W X Y Z A B C
~~~

예를 들어서, 이 방법대로 단어 'JOI'를 카이사르 단어 형식으로 변환한다면 'MRL'을 얻을 수 있고, 앞의 예와 같은 방법으로 얻은 카이사르 단어 'FURDWLD'를 원래 단어로 고치면 'CROATIA'가 된다.

출처 : https://www.acmicpc.net/problem/5598

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println(decryptCaesarCipher(scanner.nextLine()));
    }

    static String decryptCaesarCipher(String cipher) {
        StringBuilder result = new StringBuilder();
        for (char character : cipher.toCharArray()) {
            result.append(decryptCipherCharacter(character));
        }
        return result.toString();
    }

    static String decryptCipherCharacter(char character) {
        int uppercaseOrder = character - 'A';
        return Character.toString((uppercaseOrder - 3 + 26) % 26 + 'A');
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
    void decryptCaesarCipherTest() {
        assertEquals("A", decryptCaesarCipher("D"));
        assertEquals("B", decryptCaesarCipher("E"));
        assertEquals("C", decryptCaesarCipher("F"));
        assertEquals("AA", decryptCaesarCipher("DD"));
        assertEquals("JOI", decryptCaesarCipher("MRL"));
        assertEquals("CROATIA", decryptCaesarCipher("FURDWLD"));
        assertEquals("ZZZ", decryptCaesarCipher("CCC"));
        assertEquals("ZYX", decryptCaesarCipher("CBA"));
    }

    private String decryptCaesarCipher(String cipher) {
        StringBuilder result = new StringBuilder();
        for (char character : cipher.toCharArray()) {
            result.append(decryptCipherCharacter(character));
        }
        return result.toString();
    }

    private String decryptCipherCharacter(char character) {
        int uppercaseOrder = character - 'A';
        return Character.toString((uppercaseOrder - 3 + 26) % 26 + 'A');
    }
}
~~~
