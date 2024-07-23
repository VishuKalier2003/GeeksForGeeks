### Undirected Graph Cycle

**Question Description**

Given an undirected graph with \( V \) vertices labeled from \( 0 \) to \( V-1 \) and \( E \) edges, check whether it contains any cycle or not. The graph is in the form of an adjacency list where `adj[i]` contains all the nodes the \(i\)-th node is connected to.

### Intuition

To detect cycles in an undirected graph, we can use Depth-First Search (DFS). The main idea is to traverse the graph using DFS and keep track of the visited nodes. During the traversal, if we encounter a node that has already been visited and it is not the parent of the current node, then there is a cycle in the graph.

### Code and Explanation

#### Class and Main Function

```java
class Solution {
    // Function to detect cycle in an undirected graph.
    public boolean isCycle(int V, ArrayList<ArrayList<Integer>> adj) {
        boolean vis[] = new boolean[V];
        
        // Iterate through all vertices of the graph
        for(int i = 0; i < V; i++) {
            // If a vertex is not visited, check for a cycle starting from that vertex
            if(vis[i] == false) {
                if(checkForCycle(i, -1, vis, adj))
                    return true; 
            }
        }
        
        // If no cycle is found in any vertex, return false
        return false; 
    }
}
```

**Explanation:**

- **isCycle Function**:
  - Initializes a visited array to keep track of visited nodes.
  - Iterates through all vertices and checks for a cycle starting from each unvisited vertex using the `checkForCycle` function.
  - Returns `true` if any cycle is detected, otherwise returns `false`.

#### Check for Cycle Function

```java
Boolean checkForCycle(int node, int parent, boolean vis[], ArrayList<ArrayList<Integer>> adj) {
    // Mark the current node as visited
    vis[node] = true; 
    
    // Traverse through all adjacent nodes
    for(Integer it: adj.get(node)) {
        // If the adjacent node is not visited, recursively check for a cycle
        if(vis[it] == false) {
            if(checkForCycle(it, node, vis, adj) == true) 
                return true; 
        }
        // If the adjacent node is visited and not the parent of the current node, there is a cycle
        else if(it != parent) 
            return true; 
    }
    
    // If no cycle is found, return false
    return false; 
}
```

**Explanation:**

- **checkForCycle Function**:
  - Marks the current node as visited.
  - Iterates through all adjacent nodes.
  - If an adjacent node is not visited, recursively checks for a cycle from that node.
  - If an adjacent node is visited and it is not the parent of the current node, then a cycle is detected.
  - Returns `true` if a cycle is found, otherwise returns `false`.

### Complexity

- **Time Complexity**: \( O(V + E) \)
  - Each vertex and edge is processed once during the DFS traversal.

- **Space Complexity**: \( O(V) \)
  - Space is required for the visited array and the recursion stack in the worst case scenario (when the graph is a tree).
