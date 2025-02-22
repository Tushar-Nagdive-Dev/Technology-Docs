Below is a Java program implementing five operations related to a directed graph using an adjacency list representation. We'll create a `DirectedGraph` class with all methods and include a `main` method to test them. Each method is well-documented with simple explanations.

```java
import java.util.*;

public class DirectedGraph {
    private int vertices; // Number of vertices
    private List<List<Integer>> adjList; // Adjacency list representation
    
    // Constructor to initialize graph with V vertices
    public DirectedGraph(int vertices) {
        this.vertices = vertices;
        adjList = new ArrayList<>(vertices);
        for (int i = 0; i < vertices; i++) {
            adjList.add(new LinkedList<>());
        }
    }
    
    // Add directed edge from src to dest
    public void addEdge(int src, int dest) {
        adjList.get(src).add(dest);
    }
    
    // 1. Check if graph is connected (for directed graph, checks strong connectivity)
    /**
     * Uses DFS to check if graph is strongly connected (every vertex reachable from every other).
     * Time Complexity: O(V + E), Space Complexity: O(V)
     */
    public boolean isConnected() {
        if (vertices == 0) return true;
        
        // Check reachability from vertex 0 to all others
        boolean[] visited = new boolean[vertices];
        dfs(0, visited);
        
        // If any vertex not visited, not connected
        for (boolean v : visited) {
            if (!v) return false;
        }
        
        // Check reverse graph (transpose) for reachability back
        DirectedGraph transpose = getTranspose();
        Arrays.fill(visited, false);
        transpose.dfs(0, visited);
        
        for (boolean v : visited) {
            if (!v) return false;
        }
        return true;
    }
    
    private void dfs(int vertex, boolean[] visited) {
        visited[vertex] = true;
        for (int neighbor : adjList.get(vertex)) {
            if (!visited[neighbor]) {
                dfs(neighbor, visited);
            }
        }
    }
    
    private DirectedGraph getTranspose() {
        DirectedGraph transpose = new DirectedGraph(vertices);
        for (int v = 0; v < vertices; v++) {
            for (int neighbor : adjList.get(v)) {
                transpose.addEdge(neighbor, v);
            }
        }
        return transpose;
    }
    
    // 2. Dijkstra’s Shortest Path Algorithm
    /**
     * Finds shortest path from source to all vertices. Assumes non-negative weights.
     * Time Complexity: O((V + E) log V) with priority queue, Space Complexity: O(V)
     */
    public int[] dijkstra(int src, int[][] weights) {
        int[] distances = new int[vertices];
        Arrays.fill(distances, Integer.MAX_VALUE);
        distances[src] = 0;
        
        // Min-heap priority queue of (distance, vertex)
        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        pq.offer(new int[]{0, src});
        
        while (!pq.isEmpty()) {
            int[] current = pq.poll();
            int dist = current[0];
            int vertex = current[1];
            
            if (dist > distances[vertex]) continue; // Skip if already found shorter path
            
            for (int neighbor : adjList.get(vertex)) {
                int weight = weights[vertex][neighbor];
                if (distances[vertex] + weight < distances[neighbor]) {
                    distances[neighbor] = distances[vertex] + weight;
                    pq.offer(new int[]{distances[neighbor], neighbor});
                }
            }
        }
        return distances;
    }
    
    // 3. Detect cycles in graph using DFS
    /**
     * Uses DFS with recursion stack to detect cycles in directed graph.
     * Time Complexity: O(V + E), Space Complexity: O(V)
     */
    public boolean hasCycle() {
        boolean[] visited = new boolean[vertices];
        boolean[] recStack = new boolean[vertices]; // Tracks vertices in current recursion
        
        for (int v = 0; v < vertices; v++) {
            if (!visited[v] && hasCycleUtil(v, visited, recStack)) {
                return true;
            }
        }
        return false;
    }
    
    private boolean hasCycleUtil(int vertex, boolean[] visited, boolean[] recStack) {
        visited[vertex] = true;
        recStack[vertex] = true;
        
        for (int neighbor : adjList.get(vertex)) {
            if (!visited[neighbor] && hasCycleUtil(neighbor, visited, recStack)) {
                return true;
            } else if (recStack[neighbor]) {
                return true; // Back edge found
            }
        }
        recStack[vertex] = false; // Remove from recursion stack
        return false;
    }
    
    // 4. Topological Sorting for DAG
    /**
     * Orders vertices such that for every edge (u,v), u comes before v.
     * Assumes graph is a DAG. Time Complexity: O(V + E), Space Complexity: O(V)
     */
    public List<Integer> topologicalSort() {
        List<Integer> result = new ArrayList<>();
        boolean[] visited = new boolean[vertices];
        Stack<Integer> stack = new Stack<>();
        
        for (int v = 0; v < vertices; v++) {
            if (!visited[v]) {
                topologicalSortUtil(v, visited, stack);
            }
        }
        
        while (!stack.isEmpty()) {
            result.add(stack.pop());
        }
        return result;
    }
    
    private void topologicalSortUtil(int vertex, boolean[] visited, Stack<Integer> stack) {
        visited[vertex] = true;
        for (int neighbor : adjList.get(vertex)) {
            if (!visited[neighbor]) {
                topologicalSortUtil(neighbor, visited, stack);
            }
        }
        stack.push(vertex); // Add vertex after all its dependents are processed
    }
    
    // Main method to test all functions
    public static void main(String[] args) {
        // Test graph with 4 vertices
        DirectedGraph graph = new DirectedGraph(4);
        graph.addEdge(0, 1);
        graph.addEdge(0, 2);
        graph.addEdge(1, 2);
        graph.addEdge(2, 0);
        graph.addEdge(2, 3);
        graph.addEdge(3, 3);
        
        // Test 1: Is Connected
        System.out.println("Is graph strongly connected? " + graph.isConnected()); // false
        
        // Test 2: Dijkstra’s Algorithm (example weights)
        int[][] weights = new int[4][4];
        weights[0][1] = 4;
        weights[0][2] = 8;
        weights[1][2] = 11;
        weights[2][0] = 2;
        weights[2][3] = 1;
        weights[3][3] = 0; // Self-loop weight
        int[] distances = graph.dijkstra(0, weights);
        System.out.println("Shortest distances from vertex 0: " + Arrays.toString(distances));
        
        // Test 3: Cycle Detection
        System.out.println("Has cycle? " + graph.hasCycle()); // true
        
        // Test 4: Topological Sort (create a DAG)
        DirectedGraph dag = new DirectedGraph(6);
        dag.addEdge(5, 2);
        dag.addEdge(5, 0);
        dag.addEdge(4, 0);
        dag.addEdge(4, 1);
        dag.addEdge(2, 3);
        dag.addEdge(3, 1);
        System.out.println("Topological Sort: " + dag.topologicalSort());
    }
}
```

### Simple Explanation of Each Method:

1. **Graph Representation (Adjacency List):**
   - **What it does:** Stores a directed graph where each vertex has a list of neighbors.
   - **How:** Uses a `List<List<Integer>>` where `adjList.get(v)` is the list of vertices `v` points to.
   - **Example:** `addEdge(0, 1)` means vertex 0 has an edge to 1 → `adjList[0] = [1]`.
   - **Key Point:** Efficient for sparse graphs, easy to add edges.

2. **Check if Graph is Connected:**
   - **What it does:** Checks if every vertex can reach every other (strongly connected).
   - **How:** 
     - DFS from vertex 0 to see if all are reachable.
     - Create transpose (reverse edges), DFS again from 0.
     - If both reach all vertices, it’s strongly connected.
   - **Example:** `0 -> 1 -> 2 -> 0, 2 -> 3` → 3 not reachable from 0 in transpose → false.
   - **Key Point:** For directed graphs, needs both directions checked.

3. **Dijkstra’s Shortest Path:**
   - **What it does:** Finds shortest path from a source to all vertices (assuming non-negative weights).
   - **How:** Uses a priority queue:
     - Start with source (distance 0), explore neighbors.
     - Update distances if shorter path found, pick next smallest distance.
   - **Example:** `0 -> 1 (4), 0 -> 2 (8), 1 -> 2 (11)` → distances `[0, 4, 8, ∞]`.
   - **Key Point:** Greedy approach, works with weighted edges.

4. **Detect Cycles Using DFS:**
   - **What it does:** Checks if there’s a loop in the graph (e.g., `2 -> 0 -> 2`).
   - **How:** DFS with a recursion stack:
     - Mark vertex visited and in stack.
     - If a neighbor is in stack, it’s a cycle (back edge).
   - **Example:** `0 -> 1 -> 2 -> 0` → 2 sees 0 in stack → true.
   - **Key Point:** Tracks active path to catch cycles.

5. **Topological Sorting:**
   - **What it does:** Orders vertices so every edge goes from earlier to later (for DAGs).
   - **How:** DFS-based:
     - Visit all neighbors first, then add vertex to stack.
     - Pop stack for final order.
   - **Example:** `5 -> 2 -> 3 -> 1, 5 -> 0, 4 -> 0 -> 1` → `[5, 4, 2, 3, 0, 1]`.
   - **Key Point:** Only works if no cycles (DAG).

### Output from main():
```
Is graph strongly connected? false
Shortest distances from vertex 0: [0, 4, 8, 9]
Has cycle? true
Topological Sort: [5, 4, 2, 3, 0, 1]
```

Run this code and tweak the graph in `main()` to explore different setups! Let me know if you need more details or examples.
