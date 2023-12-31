# Greedy Algorithm(그리디 알고리즘)

## 정의

- 최적의 값을 구해야 하는 상황에서 사용되는 근시안적인 방법론으로 **'각 단계에서 최적이라고 생각되는 것을 선택'** 해 나가는 방식으로 진행하여 
 최종적인 해답에 도달하는 알고리즘이다.
- 다시말해, 현 상황에서 가장 좋은 것만 선택하는 방식으로 문제를 해결하는 방식이다. 


### 활용
- 코딩테스트에서 만날 경우 사전에 외우고 있지 않아도 풀 수 있는 가능성이 높은 문제 유형
- 창의력을 요함
- 반드시 풀어야 함


## 문제 예제

### 예제1) 동전 문제
- Q1. 지불해야 하는 값이 4720일 때, 1원, 50원, 100원, 500원 동전으로 동전의 수가 가장 적게 지불하게끔 구현해보아라. (Java 코드 구현)

```java
public class GreedyCoin {

    public static void main(String[] args) {

        // 동전의 종류를 저장하는 ArrayList를 생성하고, 500원, 100원, 50원, 10원을 요소로 추가
        ArrayList<Integer> coinList = new ArrayList<Integer>(Arrays.asList(500,100,50,10));
        // 각 동전별로 사용된 개수를 저장할 ArrayList를 생성합니다.
        ArrayList<Integer> details = new ArrayList<Integer>();

        // 바꿔야 할 금액
        int price = 4720;
        // 사용된 총 동전의 개수
        int totalCoinCount = 0;
        // 특정 동전이 사용된 개수
        int coinNum = 0;
        
        for(int i=0; i< coinList.size(); i ++){
            // 현재 동전으로 바꿀 수 있는 최대 개수 (가격을 동전의 값으로 나눈 몫).
            coinNum = price / coinList.get(i);
            // 총 동전 개수에 현재 동전 개수를 더함
            totalCoinCount += coinNum;
            // 가격에서 현재 동전의 개수와 가치를 곱한 값을 빼서 남은 금액을 업데이트
            price -= coinNum * coinList.get(i);
            // 현재 동전의 사용된 개수를 details 리스트에 추가
            details.add(coinNum);
            
            System.out.println(coinList.get(i) + "원: " + coinNum + "개");
        }

        // 최종적으로 사용된 총 동전 개수를 출력.
        System.out.println("총 동전 갯수:" + totalCoinCount);
    }
}
```



출처: https://jeongkyun-it.tistory.com/197 [나의 과거일지:티스토리]



