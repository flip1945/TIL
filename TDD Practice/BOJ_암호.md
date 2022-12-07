# 암호 (Bronze 2)

### 문제 설명

Vigenere cipher이라는 암호화 방법은 암호화하려는 문장 (평문)의 단어와 암호화 키를 숫자로 바꾼 다음, 평문의 단어에 해당하는 숫자에 암호 키에 해당하는 숫자를 더하는 방식이다. 이 방법을 변형하여 평문의 단어에 암호화 키에 해당하는 숫자를 빼서 암호화하는 방식을 생각해 보자.

예를 들어 암호화 키가 love이고, 암호화할 문장이 “nice day” 라면 다음과 같이 암호화가 이루어진다.

<img src='https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/201005/dkagh.PNG'>

제시된 평문의 첫 번째 문자인 ‘n’은 해당 암호화 키 ‘l’의 알파벳 순서가 12 이므로 알파벳상의 순서에서 ‘n’보다 12앞의 문자인 ‘b’로 변형된다.

변형된 문자가 ‘a' 이전의 문자가 되면 알파벳 상에서 맨 뒤로 순서를 돌린다. 예를 들면 평문의 세 번째 문자인‘c’는 알파벳 상에서 3 번째이고 대응하는 암호화키 ‘v'는 알파벳 순서 22로 ‘c'에서 22 앞으로 당기면 ‘a'보다 훨씬 앞의 문자이어야 하는데, ‘a’앞의 문자가 없으므로 ‘z’로 돌아가 반복되어 ‘g’가 된다. 즉 평문의 문자를 암호화키의 문자가 알파벳 상에서 차지하는 순서만큼 앞으로 뺀 것으로 암호화한다.

평문의 문자가 공백 문자인 경우는 그 공백 문자를 그대로 출력한다.

이와 같은 암호화를 행하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 평문이, 둘째 줄에 암호화 키가 주어진다.

평문은 알파벳 소문자와 공백문자(space)로만 구성되며, 암호화 키는 알파벳 소문자만으로 구성된다. 평문의 길이는 공백까지 포함해서 30000자 이하이다.

---

#### 출력

첫 번째 줄에 암호문을 출력한다.

---

#### 예제 입력 1

~~~
nice day
love
~~~

#### 예제 출력 1

~~~
btgz oet
~~~

출처 : https://www.acmicpc.net/problem/1718

---

### 문제풀이

이 문제는 구현 문제입니다. 

간단한 구현 문제로 TDD를 사용해 문제를 풀었습니다.

---

#### 나의 풀이

##### 풀이
~~~java
package com.algorithm;

import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String plainText = scanner.nextLine();
        String key = scanner.nextLine();

        System.out.println(getEncryptedText(plainText, key));
    }

    private static String getEncryptedText(String plainText, String key) {
        StringBuilder result = new StringBuilder();
        String encryptionKey = getEncryptionKey(key, plainText.length());
        for (int i = 0; i < plainText.length(); i++) {
            result.append(encrypt(plainText.charAt(i), encryptionKey.charAt(i)));
        }
        return result.toString();
    }

    private static  char encrypt(char plain, char key) {
        if (plain == ' ') {
            return ' ';
        }
        return convertIntToLowercase(getEncryptionNumber(plain, key));
    }

    private static  int getEncryptionNumber(char plain, char key) {
        int diff = plain - key;
        return (diff > 0) ? diff : (26 + diff);
    }

    private static  String getEncryptionKey(String key, int encryptionKeySize) {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < encryptionKeySize; i++) {
            result.append(getEncryptionKeyChar(key, i));
        }
        return result.toString();
    }

    private static  char getEncryptionKeyChar(String key, int i) {
        return key.charAt(i % key.length());
    }

    private static  char convertIntToLowercase(int number) {
        return (char)(number + 96);
    }
}
~~~

##### 테스트 코드
~~~java
package com.algorithm;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @Test
    void convertTest() {
        String a = "a";
        String c = "c";
        int num1 = 1;
        int num2 = 3;
        int num3 = 26;

        assertEquals('a', convertIntToLowercase(num1));
        assertEquals('c', convertIntToLowercase(num2));
        assertEquals('z', convertIntToLowercase(num3));
    }

    @Test
    void inputTest() {
        String key1 = "love";
        String key2 = "testkey";
        String plainText1 = "not";
        String plainText2 = "nice";
        String plainText3 = "encryption";
        String plainText4 = "nice day";

        assertEquals("lov", getEncryptionKey(key1, plainText1.length()));
        assertEquals("love", getEncryptionKey(key1, plainText2.length()));
        assertEquals("lovelovelo", getEncryptionKey(key1, plainText3.length()));
        assertEquals("lovelove", getEncryptionKey(key1, plainText4.length()));

        assertEquals("tes", getEncryptionKey(key2, plainText1.length()));
        assertEquals("test", getEncryptionKey(key2, plainText2.length()));
        assertEquals("testkeytes", getEncryptionKey(key2, plainText3.length()));
        assertEquals("testkeyt", getEncryptionKey(key2, plainText4.length()));
    }

    @Test
    void encryptionTest() {
        String key1 = "a";
        String key2 = "b";
        String plainText1 = "c";
        String plainText2 = "z";
        String plainText3 = "a";
        String plainText4 = " ";

        assertEquals('b', encrypt(plainText1.charAt(0), key1.charAt(0)));
        assertEquals('y', encrypt(plainText2.charAt(0), key1.charAt(0)));
        assertEquals('z', encrypt(plainText3.charAt(0), key1.charAt(0)));
        assertEquals(' ', encrypt(plainText4.charAt(0), key1.charAt(0)));
        assertEquals('y', encrypt(plainText3.charAt(0), key2.charAt(0)));
    }

    @Test
    void integrationTest() {
        String key1 = "love";
        String key2 = "aaa";
        String plainText1 = "nice day";
        String plainText2 = "ccc";

        assertEquals("btgz oet", getEncryptedText(key1, plainText1));
        assertEquals("bbb", getEncryptedText(key2, plainText2));
    }

    private String getEncryptedText(String key, String plainText) {
        StringBuilder result = new StringBuilder();
        String encryptionKey = getEncryptionKey(key, plainText.length());
        for (int i = 0; i < plainText.length(); i++) {
            result.append(encrypt(plainText.charAt(i), encryptionKey.charAt(i)));
        }
        return result.toString();
    }

    private char encrypt(char plain, char key) {
        if (plain == ' ') {
            return ' ';
        }
        return convertIntToLowercase(getEncryptionNumber(plain, key));
    }

    private int getEncryptionNumber(char plain, char key) {
        int diff = plain - key;
        return (diff > 0) ? diff : (26 + diff);
    }

    private String getEncryptionKey(String key, int encryptionKeySize) {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < encryptionKeySize; i++) {
            result.append(getEncryptionKeyChar(key, i));
        }
        return result.toString();
    }

    private char getEncryptionKeyChar(String key, int i) {
        return key.charAt(i % key.length());
    }

    private char convertIntToLowercase(int number) {
        return (char)(number + 96);
    }
}
~~~
