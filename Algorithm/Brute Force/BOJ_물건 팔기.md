# 물건 팔기 (Silver 4)

### 문제

세준이는 오랜 연구기간 끝에 신상품을 내놓았다. 세준이는 오랜 시간이 걸린 만큼 이 상품을 최대 이익에 팔려고 한다.

세준이는 이 상품을 사려고 하는 사람들이 총 몇 명이나 되는지 알아봤다. 무려 N명이나 살 의향을 보였다. 각각의 사람은 자기가 지불할 생각이 있는 최대 한도가 있다. 따라서, 어떤 사람이 20원까지 지불할 생각이 있는데, 세준이가 가격을 30원으로 책정하면 이 사람은 절대 안 살 것이고, 15원으로 책정하면 이 사람은 이 상품을 15원에 살 것이다. (단, 세준이가 안 팔수도 있다.)

그리고, 세준이는 각각의 사람에게 배달하는 비용이 얼마나 걸리는 지 알고 있다.

N명의 사람과, 각각의 사람이 지불할 용의가 있는 최대 금액과 배송비가 주어졌을 때, 세준이의 이익을 최대로 하는 가격을 출력하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 세준이의 물건을 구매할 의향이 있는 사람의 수 N이 주어진다. 이 값은 50보다 작거나 같다. 둘째 줄부터 각 사람이 지불할 최대 금액과 배송비가 공백을 사이에 두고 주어진다. 두 값은 모두 106보다 작거나 같은 음이 아닌 정수이고, 배송비는 0이 될 수도 있다.

---

#### 출력

첫째 줄에 최대 이익을 만들어주는 가격을 출력한다. 이익이 최대인 가격이 여러개라면, 가장 낮은 가격을 출력한다. 또, 어떤 가격으로 팔아도 이익을 남길 수 없다면 0을 출력한다.

출처 : https://www.acmicpc.net/problem/1487

---

### 문제풀이

이번 문제는 완전 탐색 문제입니다.

처음에 문제 이해를 잘못해서 한 명에게만 물건을 팔 수 있는 것으로 풀었다가 예제를 보고 잘못 이해 했다는 걸 깨달았습니다.

이 문제는 특정 가격에서 물건을 팔았을 때 여러명에게 물건을 판 가격의 최대가 되는 가격이 얼마인지 구하는 문제입니다.

팔 수 있는 모든 가격을 탐색하면서 최대 이득이 되는 가격이 얼마인지 구하기만 하면 되는 문제입니다.

---

#### 나의 풀이

~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<Customer> customers = IntStream.range(0, n)
                .mapToObj(i -> new Customer(scanner.nextInt(), scanner.nextInt()))
                .sorted(Comparator.comparing(Customer::getPayablePrice))
                .collect(Collectors.toList());

        Set<Integer> prices = customers.stream()
                .map(Customer::getPayablePrice)
                .collect(Collectors.toCollection(TreeSet::new));

        int maxProfit = 0;
        int maxProfitPrice = 0;

        for (Integer price : prices) {
            int totalProfit = 0;
            for (Customer customer : customers) {
                if (customer.getPayablePrice() < price) {
                    continue;
                }
                totalProfit += Math.max(price - customer.getDeliveryFee(), 0);
            }

            if (totalProfit > maxProfit) {
                maxProfit = totalProfit;
                maxProfitPrice = price;
            }
        }

        System.out.println(maxProfitPrice);
    }
}

class Customer {
    private final int payablePrice;
    private final int deliveryFee;

    public Customer(int payablePrice, int deliveryFee) {
        this.payablePrice = payablePrice;
        this.deliveryFee = deliveryFee;
    }

    public int getPayablePrice() {
        return payablePrice;
    }

    public int getDeliveryFee() {
        return deliveryFee;
    }
}
~~~
