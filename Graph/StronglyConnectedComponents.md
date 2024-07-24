### Bridges in a Graph

**Question Description**

Given an undirected graph of \( V \) vertices and \( E \) edges, the task is to find all the bridges in the graph. A bridge (or cut-edge) is an edge that, when removed, increases the number of connected components in the graph.

### Intuition

To identify bridges, we can use Tarjan's algorithm. The idea is to use Depth First Search (DFS) to explore the graph while maintaining discovery times of visited vertices and the lowest discovery time reachable from each vertex. An edge (u, v) is a bridge if there is no other way to reach u or v through any other path.

### Code and Explanation

#### Class and Main Function

```java
public class Solution {
    private static int time = 0;

    public static List<List<Integer>> findBridges(int[][] edges, int v, int e) {
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int i = 0; i < v; i++) {       // Defining the map...
            graph.put(i, new ArrayList<>());
        }
        for (int[] edge : edges) {
            // Filling the vertices of the edge in the graph...
            int start = edge[0], end = edge[1];
            graph.get(start).add(end);
            graph.get(end).add(start);
        }

        boolean[] visited = new boolean[v];     // Array to mark visited nodes...
        int[] disc = new int[v];    // Marking value of discovery nodes...
        int[] low = new int[v];     // Marking low link values of each node...
        int[] parent = new int[v];  // Storing parent of each node (we can use a global Stack instead)...
        List<List<Integer>> bridges = new ArrayList<>();

        Arrays.fill(parent, -1);    // Base evaluation of arrays...

        for (int i = 0; i < v; i++) {
            if (!visited[i]) {      // If the node is not visited...
                dfs(graph, i, visited, disc, low, parent, bridges);
            }
        }
        return bridges;
    }
}
```

**Explanation:**

- **findBridges Function**:
  - Initializes the adjacency list.
  - Fills the adjacency list based on the given edges.
  - Initializes necessary arrays to track visited nodes, discovery times, low-link values, and parent nodes.
  - Iterates through each vertex, invoking DFS for unvisited vertices.

#### Depth First Search (DFS) Function

```java
private static void dfs(Map<Integer, List<Integer>> graph, int u, boolean[] visited, int[] disc, int[] low, int[] parent, List<List<Integer>> bridges) {
    visited[u] = true;      // Mark current node as visited...
    disc[u] = low[u] = ++time;      // Update the time as low-link value...
    for (int v : graph.get(u)) {    
        if (!visited[v]) {
            parent[v] = u;      // For every node update the parent...
            dfs(graph, v, visited, disc, low, parent, bridges);     // DFS...

            low[u] = Math.min(low[u], low[v]);      // Updating the low link value...

            if (low[v] > disc[u]) {     // If the node low link is lesser than it is a bridge...
                bridges.add(Arrays.asList(u, v));
            }
        } else if (v != parent[u]) {    // If the node is visited, update the low link...
            low[u] = Math.min(low[u], disc[v]);     // Low link updated by current and parent...
        }
    }
}
```

**Explanation:**

- **dfs Function**:
  - Marks the current node as visited.
  - Sets the discovery and low-link values.
  - Iterates through each adjacent node.
  - For unvisited neighbors, recursively performs DFS.
  - Updates the low-link value of the current node.
  - Identifies and records bridges if the condition `low[v] > disc[u]` is met.
  - For visited neighbors, updates the low-link value if the neighbor is not the parent.

### Complexity

- **Time Complexity**: \(O(V + E)\)
  - Each vertex and edge is processed once during the DFS traversal.

- **Space Complexity**: \(O(V + E)\)
  - Space is required for the adjacency list, recursion stack, and various arrays.

This implementation ensures efficient identification of bridges in an undirected graph using DFS and low-link values.
