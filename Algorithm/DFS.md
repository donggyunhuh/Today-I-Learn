# DFS (Depth-First Search) 알고리즘 (Java 구현)

## 개념

DFS는 그래프의 모든 노드를 탐색하는 알고리즘입니다. 가능한 한 깊게 그래프를 탐색한 후, 다른 경로를 탐색합니다.

## Java에서의 구현

### 1. 재귀를 사용한 DFS 구현

재귀 방식은 현재 노드를 방문 처리한 후, 인접한 노드에 대해 같은 함수를 재귀적으로 호출합니다.

```java
import java.util.*;

public class DFS {
    private HashSet<String> visited;
    private HashMap<String, LinkedHashSet<String>> graph;

    public DFS() {
        visited = new HashSet<>();
        graph = new HashMap<>();
    }

    public void addEdge(String node1, String node2) {
        LinkedHashSet<String> adjacent = graph.getOrDefault(node1, new LinkedHashSet<>());
        adjacent.add(node2);
        graph.put(node1, adjacent);
    }

    public void dfsRecursive(String node) {
        visited.add(node);
        System.out.println(node);
        for (String neighbor : graph.getOrDefault(node, new LinkedHashSet<>())) {
            if (!visited.contains(neighbor)) {
                dfsRecursive(neighbor);
            }
        }
    }
}
```

### 2. 스택을 사용한 DFS 구현
스택을 사용하는 방식은 후입선출(LIFO) 원칙의 스택 자료 구조를 사용합니다.

```java
Copy code
public void dfsStack(String startNode) {
    Stack<String> stack = new Stack<>();
    stack.push(startNode);

    while (!stack.isEmpty()) {
        String node = stack.pop();

        if (!visited.contains(node)) {
            visited.add(node);
            System.out.println(node);

            for (String neighbor : graph.getOrDefault(node, new LinkedHashSet<>())) {
                if (!visited.contains(neighbor)) {
                    stack.push(neighbor);
                }
            }
        }
    }
}
```