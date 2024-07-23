### Hamiltonian Path

**Question Description**

A Hamiltonian path in an undirected graph is a path that visits each vertex exactly once. The task is to check if such a path exists in a given undirected graph.

### Intuition

To determine if a Hamiltonian path exists in a given graph, we can use backtracking. The idea is to start from each vertex and attempt to visit every other vertex exactly once. If we manage to visit all vertices from any starting point, a Hamiltonian path exists.

### Code and Explanation

#### Class and Main Function

```java
import java.util.*;

class Solution {
    public Boolean check(int N, int M, ArrayList<ArrayList<Integer>> Edges) {
        // Adjacency List defined...
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for(int i = 1; i <= N; i++) // Initialising the Adjacency List...
            graph.put(i, new ArrayList<Integer>());
        for (ArrayList<Integer> edge : Edges) {
            int u = edge.get(0);
            int v = edge.get(1);
            graph.get(u).add(v);
            graph.get(v).add(u); // Since graph is undirected, add both directions...
        }
        boolean visited[] = new boolean[N]; // Array to mark visited nodes...
        for(int i = 1; i <= N; i++) {
            Arrays.fill(visited, false);    // Re-initialising the array for each start node...
            if(findHamiltonian(i, visited, 0, N, graph))
                return true;    // If Hamiltonian path exists from current node...
        }
        return false;       // If no node forms the Hamiltonian Path...
    }
}
```

**Explanation:**

- **check Function**:
  - Initializes the adjacency list.
  - Fills the adjacency list based on the given edges.
  - Iterates through each vertex, attempting to find a Hamiltonian path starting from that vertex using the `findHamiltonian` function.

#### Hamiltonian Path Finding Function

```java
public boolean findHamiltonian(int node, boolean visited[], int count, int n, Map<Integer, List<Integer>> graph) {
    visited[node-1] = true; // Mark current node as visited...
    if(count+1 == n)  return true;  // Checking if all nodes are marked...
    for(int neighbor : graph.get(node)) {
        if(!visited[neighbor-1]) {  // If the node is not marked...
            if(findHamiltonian(neighbor, visited, count+1, n, graph))
                return true;        // If the path gets formed...
            // Backtracking, when the neighbor node does not make the Hamiltonian Path...
            visited[neighbor-1] = false;
        }
    }
    // When any neighbor of the current node does not forms the Hamiltonia Path...
    visited[node-1] = false;        // Backtrack...
    return false;       // Return false when the path doesn't gets traced...
}
```

**Explanation:**

- **findHamiltonian Function**:
  - Uses backtracking to attempt to find a Hamiltonian path from the current node.
  - Marks the current node as visited.
  - Checks if all nodes have been visited.
  - Recursively tries to visit each unvisited neighbor.
  - If a path is found, it returns true.
  - If no path is found, it backtracks by unmarking the current node and returns false.

### Complexity

- **Time Complexity**: \(O(N!)\)
  - The worst-case scenario involves trying every possible permutation of the nodes, leading to factorial time complexity.

- **Space Complexity**: \(O(N + M)\)
  - Space is required for the adjacency list and the visited array.
