# 큐 2 (Silver 4)

### 문제 설명

정수를 저장하는 큐를 구현한 다음, 입력으로 주어지는 명령을 처리하는 프로그램을 작성하시오.

명령은 총 여섯 가지이다.

* push X: 정수 X를 큐에 넣는 연산이다.
* pop: 큐에서 가장 앞에 있는 정수를 빼고, 그 수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.
* size: 큐에 들어있는 정수의 개수를 출력한다.
* empty: 큐가 비어있으면 1, 아니면 0을 출력한다.
* front: 큐의 가장 앞에 있는 정수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.
* back: 큐의 가장 뒤에 있는 정수를 출력한다. 만약 큐에 들어있는 정수가 없는 경우에는 -1을 출력한다.

출처 : https://www.acmicpc.net/problem/18258

---

#### 풀이
~~~java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        Queue queue = new Queue();
        StringBuilder answer = new StringBuilder();

        for (int i = 0; i < n; i++) {
            executeCommand(br.readLine(), queue, answer);
        }

        System.out.println(answer);
    }

    static void executeCommand(String command, Queue queue, StringBuilder answer) {
        if (command.startsWith("push")) {
            queue.push(Integer.parseInt(command.split(" ")[1]));
        } else if (command.startsWith("pop")) {
            answer.append(queue.pop()).append("\n");
        } else if (command.startsWith("size")) {
            answer.append(queue.size()).append("\n");
        } else if (command.startsWith("empty")) {
            answer.append(queue.empty()).append("\n");
        } else if (command.startsWith("front")) {
            answer.append(queue.front()).append("\n");
        } else if (command.startsWith("back")) {
            answer.append(queue.back()).append("\n");
        }
    }
}

class Queue {
    private final List<Integer> queue = new LinkedList<>();

    public void push(int number) {
        queue.add(number);
    }

    public int pop() {
        if (queue.size() <= 0) {
            return -1;
        }
        return queue.remove(0);
    }

    public int size() {
        return queue.size();
    }

    public int empty() {
        return queue.isEmpty() ? 1 : 0;
    }

    public int front() {
        if (empty() == 1) {
            return -1;
        }
        return queue.get(0);
    }

    public int back() {
        if (empty() == 1) {
            return -1;
        }
        return queue.get(queue.size() - 1);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import java.util.LinkedList;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {
    Queue queue;

    @BeforeEach
    void setUp() {
        queue = new Queue();
    }

    @Test
    void pushAndSizeTest() {
        queue.push(1);
        queue.push(2);
        assertEquals(2, queue.size());

        queue.push(3);
        queue.push(4);
        queue.push(5);
        assertEquals(5, queue.size());
    }

    @Test
    void popTest() {
        queue.push(1);
        queue.push(2);
        assertEquals(1, queue.pop());
        assertEquals(2, queue.pop());
        assertEquals(-1, queue.pop());
        assertEquals(-1, queue.pop());
    }

    @Test
    void emptyTest() {
        assertEquals(1, queue.empty());

        queue.push(10);
        assertEquals(0, queue.empty());
    }

    @Test
    void frontTest() {
        queue.push(10);
        queue.push(15);
        queue.push(20);
        assertEquals(10, queue.front());
        assertEquals(10, queue.front());

        queue.pop();
        queue.pop();
        queue.pop();
        assertEquals(-1, queue.front());
        assertEquals(-1, queue.front());
    }

    @Test
    void backTest() {
        queue.push(1);
        queue.push(2);
        queue.push(3);
        assertEquals(3, queue.back());
        assertEquals(3, queue.back());

        queue.pop();
        queue.pop();
        queue.pop();
        assertEquals(-1, queue.back());
        assertEquals(-1, queue.back());
    }
}

class Queue {
    private final List<Integer> queue = new LinkedList<>();

    public void push(int number) {
        queue.add(number);
    }

    public int pop() {
        if (queue.size() <= 0) {
            return -1;
        }
        return queue.remove(0);
    }

    public int size() {
        return queue.size();
    }

    public int empty() {
        return queue.isEmpty() ? 1 : 0;
    }

    public int front() {
        if (empty() == 1) {
            return -1;
        }
        return queue.get(0);
    }

    public int back() {
        if (empty() == 1) {
            return -1;
        }
        return queue.get(queue.size() - 1);
    }
}
~~~
