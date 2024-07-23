### Minimum Spanning Tree (MST)

**Question Description**

Given a weighted, undirected, and connected graph with \( V \) vertices and \( E \) edges, your task is to find the sum of the weights of the edges in the Minimum Spanning Tree (MST) of the graph. The graph is represented by an adjacency list, where each element `adj[i]` is a vector containing pairs of integers. Each pair represents an edge, with the first integer denoting the endpoint of the edge and the second integer denoting the weight of the edge.

### Intuition

To find the MST of a graph, we can use Prim's Algorithm. Prim's Algorithm works by starting from an arbitrary node, then expanding the MST one edge at a time by always adding the least-weight edge that connects a node in the MST to a node outside the MST.

### Code and Explanation

#### Class and Main Function

```java
class Solution {
    static int spanningTree(int V, int E, List<List<int[]>> adj) {
        // Set to store nodes taken into a Minimum Spanning Tree...
        Set<Integer> minimumTree = new HashSet<Integer>();
        int sum = 0;        // Initial Sum of the tree...
        // minHeap to store the nodes in ascending order based on their weights a[1]...
        PriorityQueue<int[]> minHeap = new PriorityQueue<int[]>(Comparator.comparingInt(a -> a[1]));
        minimumTree.add(0);     // Add the zero node to the tree...
        for(int[] neighbor : adj.get(0))
                minHeap.add(neighbor);      // Add the neighbors to the Heap...
        // While the size of the Heap is less than total nodes (do not decrease V)...
        while(minimumTree.size() < V) {
            int[] data = minHeap.poll();    // Poll the shortest weight array...
            int weight = data[1], end = data[0];
            if(!minimumTree.contains(end)) {    // If the node is not taken...
                sum += weight;      // Add the weight to sum and node to MST set...
                minimumTree.add(end);
                // For each neighbor of the added node...
                for(int[] neighbor : adj.get(end)) {
                    // If the neighbor is not a MST node...
                    if(!minimumTree.contains(neighbor[0]))
                        minHeap.add(neighbor);      // Add it to the heap for computaton...
                }
            }
        }
        return sum;     // Return the sum of weights of Minimum Spanning Tree...
    }
}
```

**Explanation:**

1. **Initialization**:
   - `minimumTree`: A set to keep track of nodes that are already included in the MST.
   - `sum`: A variable to keep the sum of the weights of the MST edges.
   - `minHeap`: A priority queue to store edges in ascending order based on their weights.

2. **Starting Point**:
   - The algorithm starts from node `0` and adds all its neighbors to the `minHeap`.

3. **Prim's Algorithm**:
   - The algorithm continues until all vertices are included in the MST.
   - It extracts the smallest edge from the `minHeap`.
   - If the extracted edge connects to a node not already in the MST, the node is added to the MST, the weight is added to the `sum`, and all neighbors of this node are added to the `minHeap` if they are not already in the MST.

4. **Termination**:
   - The algorithm terminates when all nodes are included in the MST, and the sum of the MST weights is returned.

### Complexity

- **Time Complexity**: \( O((V + E) \log V) \)
  - Each edge insertion and extraction in the priority queue takes \( O(\log V) \), and there are \( O(E) \) edges and \( O(V) \) vertices to process.

- **Space Complexity**: \( O(V + E) \)
  - Space is required for the priority queue and the adjacency list representation of the graph.
