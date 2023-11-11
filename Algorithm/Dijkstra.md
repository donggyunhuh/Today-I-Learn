# Dijkstra Algorithm 다익스트라 알고리즘

 - 다익스트라 알고리즘은 음의 가중치(음의 간선, 음의 값)가 없는 그래프의 한 노드에서 각 모든 노드까지의 최단거리를 구하는 알고리즘

- 초기 모델은 O(V^2)의 시간복잡도를 가졌다. 이후 우선순위 큐 등을 이용한 고안된 알고리즘이 탄생
- 현재는 O((V+E)log V)의 시간복잡도를 가지고 있다

*다익스트라 알고리즘의 매커니즘*

-> 다익스트라 알고리즘은 기본적으로 그리디 알고리즘이자 다이나믹 프로그래밍 기법을 사용한 알고리즘

-> 다익스트라 알고리즘은 기본적으로 아래 두 단계를 반복하여 임의의 노드에서 각 모든 노드까지의 최단거리 구함. 
  임의의 노드에서 다른 노드로 가는 값을 비용이라고 두자.

### (1) 방문하지 않은 노드 중에서 가장 비용이 적은 노드를 선택(그리디 알고리즘).

### (2) 해당 노드로부터 갈 수 있는 노드들의 비용을 갱신(다이나믹 프로그래밍).

#### 예제) A노드에서 출발하여 F노드로 가는 최단 경로를 구하는 문제 

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQ0yim%2FbtqZSuIfKTX%2FORDr8TKshVic2HL8MZILE1%2Fimg.png">

### 각 단계마다 기준이 필요하기 때문에, 지금부터는 방문하지 않은 노드의 집합을 Before, 방문한 노드의 집합을 After, 
현재까지 A노드가 방문한 곳 까지의 최소 비용을 D[대상 노드]라고 정의하도록 하자. 그렇다면 현재 각 집합이 가지고 있는 값은 다음과 같다. 

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcIp5JD%2FbtqZSvmSfMm%2FRv4QrBbAub0KXBANFzH0VK%2Fimg.png">

### 초기화 단계

이 상태에서 방문하지 않은 노드 중 가장 비용이 적은 대상 노드는? 

A노드에서 다른 간선으로 가는 비용이 0인 간선이 존재하지 않는다면, 당연히 A노드. 

또한, 그러한 값이 존재한다고 하더라도 첫 번째 단계에서는 'A노드에서 A노드로 가는 지점이 가장 짧다'라고 그냥 정의하고 시작. 
즉, 첫 번째 단계에서는 가장 비용이 적은 노드를 선택할 기준이 없기 때문에 출발지인 A노드 자신을 초기 선택 노드로 초기화한다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkyqhF%2FbtqZP26PpBS%2FjZFwHhRPmq19Vfb7lx7dcK%2Fimg.png">

### 알고리즘 적용 단계

위 상태에서 방문하지 않은 노드 중 가장 비용이 적은 대상 노드는? 

당연히 A노드이다. 또한, A노드를 기준으로 갈 수 있는 인접 노드로의 최소 비용은 다음 그림과 같다. 

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fdxh3hK%2FbtqZQImIuPQ%2FlxaCT6jrYkkiFkDIGKhw7K%2Fimg.png">

**다음 단계**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0NDYy%2FbtqZRrkJVfN%2FCKXhckGREW1eqbys82ccFK%2Fimg.png">

A노드는 방문 했으므로 다음은 B노드 선택, B노드 방문하고 인접 노드들의 최소 비용 갱신

C노드로 가는 최소 비용은 5, F로 가는 최소 비용은 12
**다음 단계**
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F3oV4n%2FbtqZRsDVDoB%2FyGXXvLuXGk5LuPMIUnPniK%2Fimg.png">

A, B 는 방문 했으므로 다음 최소 비용인 D노드 선택,  현재까지 C노드로 가는 최소 비용이 5였기 때문에, D[C]를 4로 갱신할 수 있다. 

이전 단계와 마찬가지로, E노드로가는 비용은 미정이었기 때문에 최초 방문 값인 10으로 설정 가능하다.

**다음 단계**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbD3NB6%2FbtqZV60H070%2F2f3H3YAtoUGd5w9jgqe1Sk%2Fimg.png">

다음으로 선택된 노드는 C이고, C노드에서 E노드로 가는 값과 F노드로 가는 값은 각각 기존 E노드나 F노드로 가는 비용보다 모두 작다.

따라서 각 값을 갱신할 수 있다.

**다음 단계**
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc7fEyM%2FbtqZWNmjozq%2FgTjXlB6mRIsJuWKmCK1fA1%2Fimg.png">

다음선택은 E노드이고, E노드에서 F노드로 가는 비용은 2 이므로 총 D[F] 값은 8이 된다. 

마지막으로 방문해야하는 노드인 F에서는 더 이상 갈 수 있는 노드가 존재하지 않으므로, 방문만 완료하고 알고리즘을 종료한다. 

**다음 단계**
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FK5lmL%2FbtqZUV6qeaz%2FY6g7S9tPtkod5GPrNkElKk%2Fimg.png">

다익스트라 알고리즘에 의한 A노드부터 F노드까지(혹은 A노드부터 각 모든 정점 까지)의 최소 비용은 위와 같은 값을 가진다.



## 코드 구현

->한 번 방문한 배열은 방문할 수 없으므로 방문 여부를 체크할 배열이 필요할 것이고, 각 노드 까지의 최소 비용을 저장할 배열이 필요할 것이다.

->구현에서 고려해 주어야 하는 것은, 미방문 지점의 값을 항상 최소의 값으로 갱신하는 것이 목표이기 때문에, 미 방문 지점은 매우 큰 값으로 초기화하여 로직을 처리해주면 된다(구할 수 있다면, 노드가 가질 수 있는 최대 비용을 넣어두어도 된다).

->최소 비용을 갖는 노드을 선택하는 과정은 앞선 일반적인 구현에서는 최악의 경우 모든 노드을 확인해야 하고, 이 것은 V번 반복하기 때문에 O(V^2)의 시간 복잡도를 갖는다.

```java
package Graph;

import java.util.ArrayList;
import java.util.Scanner;

/*
sample input
5 6
1
5 1 1
1 2 2
1 3 3
2 3 4
2 4 5
3 4 6
 */

// 도착 지점과, 도착지점으로 가는 비용을 의미하는 클래스를 정의한다.
class Node {
	int idx;
	int cost;

	// 생성자
	Node(int idx, int cost) {
		this.idx = idx;
		this.cost = cost;
	}
}

public class Dijkstra {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		// 노드와 간선의 개수
		int V = sc.nextInt();
		int E = sc.nextInt();
		// 출발지점
		int start = sc.nextInt();

		// 1. 인접리스트를 이용한 그래프 초기화
		ArrayList<ArrayList<Node>> graph = new ArrayList<ArrayList<Node>>();
		// 노드의 번호가 1부터 시작하므로, 0번 인덱스 부분을 임의로 만들어 놓기만 한다.
		for (int i = 0; i < V + 1; i++) {
			graph.add(new ArrayList<>());
		}
		// 그래프에 값을 넣는다.
		for (int i = 0; i < E; i++) {
			// a로 부터 b로 가는 값은 cost이다.
			int a = sc.nextInt();
			int b = sc.nextInt();
			int cost = sc.nextInt();

			graph.get(a).add(new Node(b, cost));
		}

		// 2. 방문 여부를 확인할 boolean 배열, start 노드부터 end 노드 까지의 최소 거리를 저장할 배열을 만든다.
		boolean[] visited = new boolean[V + 1];
		int[] dist = new int[V + 1];

		// 3. 최소 거리 정보를 담을 배열을 초기화 한다.
		for (int i = 0; i < V + 1; i++) {
			// 출발 지점 외 나머지 지점까지의 최소 비용은 최대로 지정해둔다.
			dist[i] = Integer.MAX_VALUE;
		}
		// 출발 지점의 비용은 0으로 시작한다.
		dist[start] = 0;

		// 4. 다익스트라 알고리즘을 진행한다.
		// 모든 노드을 방문하면 종료하기 때문에, 노드의 개수 만큼만 반복을 한다.
		for (int i = 0; i < V; i++) {
			// 4 - 1. 현재 거리 비용 중 최소인 지점을 선택한다.
			// 해당 노드가 가지고 있는 현재 비용.
			int nodeValue = Integer.MAX_VALUE;
			// 해당 노드의 인덱스(번호).
			int nodeIdx = 0;
			// 인덱스 0은 생각하지 않기 때문에 0부터 반복을 진행한다.
			for (int j = 1; j < V + 1; j++) {
				// 해당 노드를 방문하지 않았고, 현재 모든 거리비용 중 최솟값을 찾는다.
				if (!visited[j] && dist[j] < nodeValue) {
					nodeValue = dist[j];
					nodeIdx = j;
				}
			}
			// 최종 선택된 노드를 방문처리 한다.
			visited[nodeIdx] = true;

			// 4 - 2. 해당 지점을 기준으로 인접 노드의 최소 거리 값을 갱신한다.
			for (int j = 0; j < graph.get(nodeIdx).size(); j++) {
				// 인접 노드를 선택한다.
				Node adjNode = graph.get(nodeIdx).get(j);
				// 인접 노드가 현재 가지는 최소 비용과
				// 현재 선택된 노드의 값 + 현재 노드에서 인접 노드로 가는 값을 비교하여 더 작은 값으로 갱신한다.
				if (dist[adjNode.idx] > dist[nodeIdx] + adjNode.cost) {
					dist[adjNode.idx] = dist[nodeIdx] + adjNode.cost;
				}
			}
		}

		// 5. 최종 비용을 출력한다.
		for (int i = 1; i < V + 1; i++) {
			if (dist[i] == Integer.MAX_VALUE) {
				System.out.println("INF");
			} else {
				System.out.println(dist[i]);
			}
		}
		sc.close();
	}
}
```


  [출처] :TH 티스토리,  https://sskl660.tistory.com/59