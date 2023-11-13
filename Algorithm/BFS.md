# BFS (Breadth-First Search) 알고리즘 (Java 구현)

## 개념

BFS는 그래프의 모든 노드를 방문하는 알고리즘입니다. 가장 가까운 노드부터 차례대로 탐색하는 방식으로, 큐(Queue)를 이용해 구현됩니다.

## Java에서의 구현

BFS는 큐를 이용하여 구현합니다. 현재 노드를 큐에 넣고, 큐에서 노드를 하나씩 꺼내어 방문 처리한 후, 해당 노드의 인접 노드를 큐에 추가합니다.

```java
import java.util.*;

public class BFS {
    private HashMap<String, LinkedHashSet<String>> graph;

    public BFS() {
        graph = new HashMap<>();
    }

    public void addEdge(String node1, String node2) {
        LinkedHashSet<String> adjacent = graph.getOrDefault(node1, new LinkedHashSet<>());
        adjacent.add(node2);
        graph.put(node1, adjacent);
    }

    public void bfs(String startNode) {
        Set<String> visited = new HashSet<>();
        Queue<String> queue = new LinkedList<>();

        queue.add(startNode);
        visited.add(startNode);

        while (!queue.isEmpty()) {
            String node = queue.poll();
            System.out.println(node);

            for (String neighbor : graph.getOrDefault(node, new LinkedHashSet<>())) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.add(neighbor);
                }
            }
        }
    }
}
```

