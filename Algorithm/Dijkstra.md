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