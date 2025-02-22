## 🚀 **Module 7: Mastering Graphs**  

Graphs are versatile data structures used to represent relationships between pairs of objects. They are widely used in social networks, navigation systems, web crawling, and many other applications. This module covers the fundamentals of graphs, different types, and essential algorithms for traversal and pathfinding.

---

## **🔥 7.1 What is a Graph?**  
- **Definition:** A graph `G` consists of a set of vertices (nodes) `V` and a set of edges `E` connecting the vertices.  
- **Notation:** `G = (V, E)`  

---

### 📘 **Graph Terminology**  
- **Vertex (Node):** A point in the graph.  
- **Edge:** A connection between two vertices.  
- **Adjacent Vertices:** Two vertices connected by an edge.  
- **Degree:** Number of edges connected to a vertex.  
- **Path:** A sequence of vertices connected by edges.  
- **Cycle:** A path that starts and ends at the same vertex.  
- **Connected Graph:** There is a path between every pair of vertices.  
- **Disconnected Graph:** At least one pair of vertices does not have a path connecting them.  

---

### 📘 **Types of Graphs**  
1. **Undirected Graph:** Edges have no direction.  
    - Example: Social networks where friendship is mutual.  
2. **Directed Graph (Digraph):** Edges have a direction.  
    - Example: Twitter followers (one-way connection).  
3. **Weighted Graph:** Edges have weights or costs.  
    - Example: Road networks with distances or costs.  
4. **Unweighted Graph:** Edges have no weights or costs.  

---

## **🔥 7.2 Graph Representation**  
### 📘 **1. Adjacency Matrix**  
- A 2D array where each cell `[i][j]` indicates the presence (and weight) of an edge from vertex `i` to vertex `j`.  
- **Space Complexity:** `O(V²)`  
- **Example:**  
    ```
       0   1   2   3
    0 [ 0   1   0   0 ]
    1 [ 1   0   1   1 ]
    2 [ 0   1   0   0 ]
    3 [ 0   1   0   0 ]
    ```

---

### 📘 **2. Adjacency List**  
- An array of lists where each vertex has a list of connected vertices.  
- **Space Complexity:** `O(V + E)`  
- **Example:**  
    ```
    0 -> 1
    1 -> 0 -> 2 -> 3
    2 -> 1
    3 -> 1
    ```

---

## **🔥 7.3 Implementing Graph in Java (Adjacency List)**  
Let's create a graph using an adjacency list and perform the following operations:  
1. Add an edge  
2. Display the graph  
3. Breadth-First Search (BFS)  
4. Depth-First Search (DFS)  

---

### 📘 **Example Code: Graph Implementation using Adjacency List**  
```java
import java.util.*;

public class Graph {
    private int vertices;
    private LinkedList<Integer>[] adjList;

    // Constructor
    public Graph(int vertices) {
        this.vertices = vertices;
        adjList = new LinkedList[vertices];
        for (int i = 0; i < vertices; i++) {
            adjList[i] = new LinkedList<>();
        }
    }

    // 1. Add Edge (Undirected Graph)
    public void addEdge(int src, int dest) {
        adjList[src].add(dest);
        adjList[dest].add(src);  // Comment this line for directed graph
    }

    // 2. Display Graph
    public void displayGraph() {
        for (int i = 0; i < vertices; i++) {
            System.out.print("Vertex " + i + ":");
            for (Integer vertex : adjList[i]) {
                System.out.print(" -> " + vertex);
            }
            System.out.println();
        }
    }

    // 3. Breadth-First Search (BFS)
    public void bfs(int start) {
        boolean[] visited = new boolean[vertices];
        Queue<Integer> queue = new LinkedList<>();
        
        visited[start] = true;
        queue.add(start);

        System.out.print("BFS: ");
        while (!queue.isEmpty()) {
            int vertex = queue.poll();
            System.out.print(vertex + " ");

            for (int neighbor : adjList[vertex]) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.add(neighbor);
                }
            }
        }
        System.out.println();
    }

    // 4. Depth-First Search (DFS)
    public void dfs(int start) {
        boolean[] visited = new boolean[vertices];
        System.out.print("DFS: ");
        dfsUtil(start, visited);
        System.out.println();
    }

    // Utility function for DFS
    private void dfsUtil(int vertex, boolean[] visited) {
        visited[vertex] = true;
        System.out.print(vertex + " ");

        for (int neighbor : adjList[vertex]) {
            if (!visited[neighbor]) {
                dfsUtil(neighbor, visited);
            }
        }
    }

    public static void main(String[] args) {
        Graph graph = new Graph(4);
        graph.addEdge(0, 1);
        graph.addEdge(0, 2);
        graph.addEdge(1, 2);
        graph.addEdge(2, 3);

        System.out.println("Graph Representation:");
        graph.displayGraph();

        graph.bfs(0);
        graph.dfs(0);
    }
}
```

---

### 📊 **Output:**  
```
Graph Representation:
Vertex 0: -> 1 -> 2
Vertex 1: -> 0 -> 2
Vertex 2: -> 0 -> 1 -> 3
Vertex 3: -> 2

BFS: 0 1 2 3 
DFS: 0 1 2 3 
```

---

### 🔥 **Explanation:**  
- **Add Edge:** Adds an edge between two vertices. For undirected graphs, the edge is added both ways.  
- **Display Graph:** Displays the adjacency list for each vertex.  
- **Breadth-First Search (BFS):**  
  - Uses a queue.  
  - Visits all nodes at the current level before moving to the next level.  
  - **Time Complexity:** `O(V + E)`  
- **Depth-First Search (DFS):**  
  - Uses a stack (or recursive call stack).  
  - Visits nodes by going as deep as possible before backtracking.  
  - **Time Complexity:** `O(V + E)`  

---

## **🔥 7.4 Graph Applications**  
- **Social Networks:** Friend suggestions, community detection.  
- **Navigation Systems:** Shortest path finding.  
- **Web Crawlers:** Crawling interconnected web pages.  
- **Recommendation Systems:** Product or movie recommendations.  

---

## **🔥 7.5 Common Graph Algorithms**  
1. **Dijkstra's Shortest Path Algorithm:** Finds the shortest path from a source to all vertices.  
2. **Floyd-Warshall Algorithm:** All pairs shortest path.  
3. **Kruskal's and Prim's Algorithms:** Minimum Spanning Tree (MST).  
4. **Topological Sorting:** Linear ordering of vertices in Directed Acyclic Graphs (DAGs).  

---

## **📝 Exercise Set:**  
1. Implement a directed graph using an adjacency list.  
2. Write a function to check if a graph is connected.  
3. Implement Dijkstra’s shortest path algorithm.  
4. Write a function to detect cycles in a graph using DFS.  
5. Implement Topological Sorting for a DAG.  

---

## 🔥 **Next: Dynamic Programming**  
Graphs are the foundation for many dynamic programming problems. Next, we will learn **Dynamic Programming** — a powerful technique for solving optimization problems efficiently.

---
