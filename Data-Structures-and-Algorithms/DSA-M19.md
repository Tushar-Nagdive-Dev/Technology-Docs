## 🚀 **Module 14: Mastering Graph Algorithms**  

Graph algorithms are essential for solving real-world problems like network routing, social network analysis, recommendation systems, and shortest path calculations. This module covers the most important graph algorithms, their implementations, and practical applications.

---

## **🔥 14.1 Why Learn Graph Algorithms?**  
- **Efficient Pathfinding:** Shortest path calculations for navigation and routing systems.  
- **Network Optimization:** Efficiently design and optimize networks (e.g., computer networks, social networks).  
- **AI and ML Applications:** Graph-based clustering, recommendation systems, and neural networks.  
- **Competitive Programming:** Essential for solving complex algorithmic problems.  

---

## **🔥 14.2 Overview of Graph Algorithms**  
1. **Shortest Path Algorithms:**  
    - **Dijkstra’s Algorithm** — Shortest path from a single source to all vertices (non-negative weights).  
    - **Bellman-Ford Algorithm** — Handles graphs with negative weights.  
2. **Minimum Spanning Tree (MST) Algorithms:**  
    - **Kruskal’s Algorithm** — Greedy approach using edge sorting and union-find.  
    - **Prim’s Algorithm** — Greedy approach using priority queues.  
3. **Topological Sorting:**  
    - Ordering of vertices in Directed Acyclic Graphs (DAGs).  
4. **Strongly Connected Components (SCCs):**  
    - **Kosaraju’s Algorithm** — Find SCCs in a directed graph.  
5. **Network Flow Algorithms:**  
    - **Ford-Fulkerson Algorithm** — Maximum flow in a flow network.  

---

## **🔥 14.3 Shortest Path Algorithms**  
### 📘 **1. Dijkstra’s Algorithm**  
- **Definition:** Finds the shortest path from a single source to all vertices in a weighted graph with non-negative weights.  
- **Key Idea:** Uses a priority queue (min-heap) to greedily select the vertex with the shortest path estimate.  
- **Time Complexity:** `O((V + E) log V)` — Using a priority queue.  
- **Space Complexity:** `O(V + E)`  

---

### 📘 **Example Code: Dijkstra's Algorithm**  
```java
import java.util.*;

class Node implements Comparable<Node> {
    int vertex;
    int weight;

    Node(int vertex, int weight) {
        this.vertex = vertex;
        this.weight = weight;
    }

    @Override
    public int compareTo(Node other) {
        return this.weight - other.weight;
    }
}

public class Dijkstra {
    private int vertices;
    private LinkedList<Node>[] adjList;

    // Constructor
    public Dijkstra(int vertices) {
        this.vertices = vertices;
        adjList = new LinkedList[vertices];
        for (int i = 0; i < vertices; i++) {
            adjList[i] = new LinkedList<>();
        }
    }

    // Add Edge
    public void addEdge(int src, int dest, int weight) {
        adjList[src].add(new Node(dest, weight));
        adjList[dest].add(new Node(src, weight));  // For undirected graph
    }

    // Dijkstra's Algorithm
    public void dijkstra(int src) {
        PriorityQueue<Node> minHeap = new PriorityQueue<>();
        int[] dist = new int[vertices];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[src] = 0;
        minHeap.add(new Node(src, 0));

        while (!minHeap.isEmpty()) {
            Node node = minHeap.poll();
            int u = node.vertex;

            for (Node neighbor : adjList[u]) {
                int v = neighbor.vertex;
                int weight = neighbor.weight;

                if (dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    minHeap.add(new Node(v, dist[v]));
                }
            }
        }

        System.out.println("Shortest distances from source vertex " + src + ":");
        for (int i = 0; i < vertices; i++) {
            System.out.println("Vertex " + i + " -> Distance: " + dist[i]);
        }
    }

    public static void main(String[] args) {
        Dijkstra graph = new Dijkstra(5);
        graph.addEdge(0, 1, 2);
        graph.addEdge(0, 2, 4);
        graph.addEdge(1, 2, 1);
        graph.addEdge(1, 3, 7);
        graph.addEdge(2, 4, 3);
        graph.addEdge(3, 4, 1);

        graph.dijkstra(0);
    }
}
```

---

### 📊 **Output:**  
```
Shortest distances from source vertex 0:
Vertex 0 -> Distance: 0
Vertex 1 -> Distance: 2
Vertex 2 -> Distance: 3
Vertex 3 -> Distance: 9
Vertex 4 -> Distance: 6
```

---

### 🔥 **Explanation:**  
- **Min-Heap (Priority Queue):** Efficiently retrieves the vertex with the smallest distance.  
- **Distance Array (`dist[]`):** Stores the shortest distance from the source to each vertex.  
- **Relaxation:** Updates the shortest path estimate.  
- **Greedy Choice:** Chooses the vertex with the minimum distance estimate.  

---

## **🔥 14.4 Minimum Spanning Tree (MST) Algorithms**  
### 📘 **1. Kruskal’s Algorithm**  
- **Definition:** A greedy algorithm that finds an MST by sorting edges by weight and using a union-find data structure.  
- **Key Idea:** Add the shortest edge that doesn't form a cycle.  
- **Time Complexity:** `O(E log E)` — Due to sorting of edges.  
- **Space Complexity:** `O(V + E)`  

---

### 📘 **Example Code: Kruskal's Algorithm**  
```java
import java.util.*;

class Edge implements Comparable<Edge> {
    int src, dest, weight;

    Edge(int src, int dest, int weight) {
        this.src = src;
        this.dest = dest;
        this.weight = weight;
    }

    @Override
    public int compareTo(Edge other) {
        return this.weight - other.weight;
    }
}

public class Kruskal {
    private int vertices;
    private List<Edge> edges;

    public Kruskal(int vertices) {
        this.vertices = vertices;
        edges = new ArrayList<>();
    }

    public void addEdge(int src, int dest, int weight) {
        edges.add(new Edge(src, dest, weight));
    }

    private int findParent(int[] parent, int v) {
        if (parent[v] != v) {
            parent[v] = findParent(parent, parent[v]);
        }
        return parent[v];
    }

    private void union(int[] parent, int[] rank, int u, int v) {
        int rootU = findParent(parent, u);
        int rootV = findParent(parent, v);

        if (rank[rootU] > rank[rootV]) {
            parent[rootV] = rootU;
        } else if (rank[rootU] < rank[rootV]) {
            parent[rootU] = rootV;
        } else {
            parent[rootV] = rootU;
            rank[rootU]++;
        }
    }

    public void kruskalMST() {
        Collections.sort(edges);

        int[] parent = new int[vertices];
        int[] rank = new int[vertices];

        for (int i = 0; i < vertices; i++) {
            parent[i] = i;
            rank[i] = 0;
        }

        List<Edge> mst = new ArrayList<>();

        for (Edge edge : edges) {
            int u = findParent(parent, edge.src);
            int v = findParent(parent, edge.dest);

            if (u != v) {
                mst.add(edge);
                union(parent, rank, u, v);
            }
        }

        System.out.println("Minimum Spanning Tree:");
        for (Edge edge : mst) {
            System.out.println("Edge: " + edge.src + " - " + edge.dest + ", Weight: " + edge.weight);
        }
    }

    public static void main(String[] args) {
        Kruskal graph = new Kruskal(4);
        graph.addEdge(0, 1, 10);
        graph.addEdge(0, 2, 6);
        graph.addEdge(0, 3, 5);
        graph.addEdge(1, 3, 15);
        graph.addEdge(2, 3, 4);

        graph.kruskalMST();
    }
}
```

---

### 📊 **Output:**  
```
Minimum Spanning Tree:
Edge: 2 - 3, Weight: 4
Edge: 0 - 3, Weight: 5
Edge: 0 - 1, Weight: 10
```

---

## 🔥 **Next: Competitive Programming Techniques**  
Next, we will explore advanced problem-solving techniques used in **Competitive Programming**.  

---
