![Graphs-Cheatsheet_1_1024x1024](https://github.com/soulnote/LeetCode/assets/71943647/d27289bf-1aa2-42de-af98-e8534c20832c)


# DFS
`Input :`

![image](https://github.com/soulnote/LeetCode/assets/71943647/0a55da47-99e0-49ce-9c5c-3a6e5894a5fc)

Output: 1 2 4 5 3
```java
import java.util.*;
class Solution {
    
    public static void dfs(int node, boolean vis[], ArrayList<ArrayList<Integer>> adj, 
    ArrayList<Integer> ls) {
        
        //marking current node as visited
        vis[node] = true;
        ls.add(node);
        
        //getting neighbour nodes
        for(Integer it: adj.get(node)) {
            if(vis[it] == false) {
                dfs(it, vis, adj, ls);
            }
        }
    }
    // Function to return a list containing the DFS traversal of the graph.
    public ArrayList<Integer> dfsOfGraph(int V, ArrayList<ArrayList<Integer>> adj) {
        //boolean array to keep track of visited vertices
        boolean vis[] = new boolean[V+1];
        vis[0] = true; 
        ArrayList<Integer> ls = new ArrayList<>();
        dfs(0, vis, adj, ls); 
        return ls; 
    }
    
    public static void main(String args[]) {

        ArrayList < ArrayList < Integer >> adj = new ArrayList < > ();
        for (int i = 0; i < 5; i++) {
            adj.add(new ArrayList < > ());
        }
        adj.get(0).add(2);
        adj.get(2).add(0);
        adj.get(0).add(1);
        adj.get(1).add(0);
        adj.get(0).add(3);
        adj.get(3).add(0);
        adj.get(2).add(4);
        adj.get(4).add(2);
        
        Solution sl = new Solution(); 
        ArrayList < Integer > ans = sl.dfsOfGraph(5, adj);
        int n = ans.size(); 
        for(int i = 0;i<n;i++) {
            System.out.print(ans.get(i)+" "); 
        }
    }
}
```
# BFS
`Input:`


![image](https://github.com/soulnote/LeetCode/assets/71943647/68d5f78d-2af7-4bc6-a70f-2a6270538cf1)


Output: 1 2 5 3 4
```java
import java.util.*;
class Solution {
    // Function to return Breadth First Traversal of given graph.
    public ArrayList<Integer> bfsOfGraph(int V, 
    ArrayList<ArrayList<Integer>> adj) {
        
        ArrayList < Integer > bfs = new ArrayList < > ();
        boolean vis[] = new boolean[V];
        Queue < Integer > q = new LinkedList < > ();

        q.add(0);
        vis[0] = true;

        while (!q.isEmpty()) {
            Integer node = q.poll();
            bfs.add(node);

            // Get all adjacent vertices of the dequeued vertex s
            // If a adjacent has not been visited, then mark it
            // visited and enqueue it
            for (Integer it: adj.get(node)) {
                if (vis[it] == false) {
                    vis[it] = true;
                    q.add(it);
                }
            }
        }

        return bfs;
    }
    
    public static void main(String args[]) {

        ArrayList < ArrayList < Integer >> adj = new ArrayList < > ();
        for (int i = 0; i < 5; i++) {
            adj.add(new ArrayList < > ());
        }
        adj.get(0).add(1);
        adj.get(1).add(0);
        adj.get(0).add(4);
        adj.get(4).add(0);
        adj.get(1).add(2);
        adj.get(2).add(1);
        adj.get(1).add(3);
        adj.get(3).add(1);
        
        Solution sl = new Solution(); 
        ArrayList < Integer > ans = sl.bfsOfGraph(5, adj);
        int n = ans.size(); 
        for(int i = 0;i<n;i++) {
            System.out.print(ans.get(i)+" "); 
        }
    }
}
```
# Number of Provinces
Problem Statement: Given an undirected graph with V vertices. We say two vertices u and v belong to a single province if there is a path from u to v or v to u. Your task is to find the number of provinces.
`Input:`


![image](https://github.com/soulnote/LeetCode/assets/71943647/8252f171-26fd-4b32-a069-bc6b4c243414)


Output: 3
```java
import java.util.*;

class Solution {
    // dfs traversal function 
    private static void dfs(int node, 
       ArrayList<ArrayList<Integer>> adjLs , 
       int vis[]) {
        vis[node] = 1; 
        for(Integer it: adjLs.get(node)) {
            if(vis[it] == 0) {
                dfs(it, adjLs, vis); 
            }
        }
    }
    static int numProvinces(ArrayList<ArrayList<Integer>> adj, int V) {
        ArrayList<ArrayList<Integer>> adjLs = new ArrayList<ArrayList<Integer>>(); 
        for(int i = 0;i<V;i++) {
            adjLs.add(new ArrayList<Integer>()); 
        }
        
        // to change adjacency matrix to list 
        for(int i = 0;i<V;i++) {
            for(int j = 0;j<V;j++) {
                // self nodes are not considered 
                if(adj.get(i).get(j) == 1 && i != j) {
                    adjLs.get(i).add(j); 
                    adjLs.get(j).add(i); 
                }
            }
        }
        int vis[] = new int[V]; 
        int cnt = 0; 
        for(int i = 0;i<V;i++) {
            if(vis[i] == 0) {
               cnt++;
               dfs(i, adjLs, vis); 
            }
        }
        return cnt; 
    }
    public static void main(String[] args)
    {

        // adjacency matrix 
        ArrayList<ArrayList<Integer> > adj = new ArrayList<ArrayList<Integer> >();

        adj.add(new ArrayList<Integer>());
        adj.get(0).add(0, 1);
        adj.get(0).add(1, 0);
        adj.get(0).add(2, 1);
        adj.add(new ArrayList<Integer>());
        adj.get(1).add(0, 0);
        adj.get(1).add(1, 1);
        adj.get(1).add(2, 0);
        adj.add(new ArrayList<Integer>());
        adj.get(2).add(0, 1);
        adj.get(2).add(1, 0);
        adj.get(2).add(2, 1);
                
        Solution ob = new Solution();
        System.out.println(ob.numProvinces(adj,3));
    }
}
```
# Detect Cycle in an Undirected Graph (using BFS)
**Intution:-**
 The intuition is that we start from a node, and start doing BFS level-wise, if somewhere down the line, we visit a single node twice, it means we came via two paths to end up at the same node. It implies there is a cycle in the graph because we know that we start from different directions but can arrive at the same node only if the graph is connected or contains a cycle, otherwise we would never come to the same node again.  
 ![image](https://github.com/soulnote/All-images/blob/main/bfscycle1.png)
 ![image](https://github.com/soulnote/All-images/blob/main/bfscycle2.png)
 ```java
import java.util.*;

class Solution
{
   static boolean checkForCycle(ArrayList<ArrayList<Integer>> adj, int s,
            boolean vis[], int parent[])
    {
       Queue<Node> q =  new LinkedList<>(); //BFS
       q.add(new Node(s, -1));
       vis[s] =true;
       
       // until the queue is empty
       while(!q.isEmpty())
       {
           // source node and its parent node
           int node = q.peek().first;
           int par = q.peek().second;
           q.remove(); 
           
           // go to all the adjacent nodes
           for(Integer it: adj.get(node))
           {
               if(vis[it]==false)  
               {
                   q.add(new Node(it, node));
                   vis[it] = true; 
               }
        
                // if adjacent node is visited and is not its own parent node
               else if(par != it) return true;
           }
       }
       
       return false;
    }
    
    // function to detect cycle in an undirected graph
    public boolean isCycle(int V, ArrayList<ArrayList<Integer>> adj)
    {
        boolean vis[] = new boolean[V];
        Arrays.fill(vis,false);
        int parent[] = new int[V];
        Arrays.fill(parent,-1);  
        
        for(int i=0;i<V;i++)
            if(vis[i]==false) 
                if(checkForCycle(adj, i,vis, parent)) 
                    return true;
    
        return false;
    }
    
    public static void main(String[] args)
    {
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < 4; i++) {
            adj.add(new ArrayList < > ());
        }
        adj.get(1).add(2);
        adj.get(2).add(1);
        adj.get(2).add(3);
        adj.get(3).add(2);
                
        Solution obj = new Solution();
        boolean ans = obj.isCycle(4, adj);
        if (ans)
            System.out.println("1");    
        else
            System.out.println("0");
    }
}

class Node {
    int first;
    int second;
    public Node(int first, int second) {
        this.first = first;
        this.second = second; 
    }
}
```
**Time Complexity**: O(N + 2E) + O(N), Where N = Nodes, 2E is for total degrees as we traverse all adjacent nodes. In the case of connected components of a graph, it will take another O(N) time.

**Space Complexity:** O(N) + O(N) ~ O(N), Space for queue data structure and visited array.

# Detect Cycle in an Undirected Graph (using DFS)
The intuition is that we start from a source and go in-depth, and reach any node that has been previously visited in the past; it means there's a cycle.
NOTE: We can call it a cycle only if the already visited node is a non-parent node because we cannot say we came to a node that was previously the parent node. 
![image](https://github.com/soulnote/All-images/blob/main/dfscycle1.png)
![image](https://github.com/soulnote/All-images/blob/main/dfscycle2.png)

```java
import java.util.*;

class Solution {
    private boolean dfs(int node, int parent, int vis[], ArrayList<ArrayList<Integer>> 
    adj) {
        vis[node] = 1; 
        // go to all adjacent nodes
        for(int adjacentNode: adj.get(node)) {
            if(vis[adjacentNode]==0) {
                if(dfs(adjacentNode, node, vis, adj) == true) 
                    return true; 
            }
            // if adjacent node is visited and is not its own parent node
            else if(adjacentNode != parent) return true; 
        }
        return false; 
    }
    // Function to detect cycle in an undirected graph.
    public boolean isCycle(int V, ArrayList<ArrayList<Integer>> adj) {
       int vis[] = new int[V]; 
       for(int i = 0;i<V;i++) {
           if(vis[i] == 0) {
               if(dfs(i, -1, vis, adj) == true) return true; 
           }
       }
       return false; 
    }
    public static void main(String[] args)
    {
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < 4; i++) {
            adj.add(new ArrayList < > ());
        }
        adj.get(1).add(2);
        adj.get(2).add(1);
        adj.get(2).add(3);
        adj.get(3).add(2);
                
        Solution obj = new Solution();
        boolean ans = obj.isCycle(4, adj);
        if (ans)
            System.out.println("1");    
        else
            System.out.println("0");
    }
```
**Time Complexity:** O(N + 2E) + O(N), Where N = Nodes, 2E is for total degrees as we traverse all adjacent nodes. In the case of connected components of a graph, it will take another O(N) time.

**Space Complexity:** O(N) + O(N) ~ O(N), Space for recursive stack space and visited array.


# Bipartite Graph 

Problem Statement: Given an adjacency list of a graph adj of V no. of vertices having 0 based index. Check whether the graph is bipartite or not.

If we are able to colour a graph with two colours such that no adjacent nodes have the same colour, it is called a bipartite graph.

Input:


![image](https://github.com/soulnote/LeetCode/assets/71943647/eeb73231-c6a8-4c5c-86f4-d19ddfdfd79c)


Output:  1

`Explanation:`


![image](https://github.com/soulnote/LeetCode/assets/71943647/8c9ea550-9f4e-41cc-aff3-b790f7e9b433)
## DFS Implementation
![image](https://github.com/soulnote/All-images/blob/main/bipartitedfs.gif)
```java
import java.util.*;

class Solution
{
    private boolean dfs(int node, int col, int color[], 
    ArrayList<ArrayList<Integer>>adj) {
        
        color[node] = col; 
        
        // traverse adjacent nodes
        for(int it : adj.get(node)) {
            // if uncoloured
            if(color[it] == -1) {
                if(dfs(it, 1 - col, color, adj) == false) return false; 
            }
            // if previously coloured and have the same colour
            else if(color[it] == col) {
                return false; 
            }
        }
        
        return true; 
    }
    public boolean isBipartite(int V, ArrayList<ArrayList<Integer>>adj)
    {
        int color[] = new int[V];
	    for(int i = 0;i<V;i++) color[i] = -1; 
	    
	    // for connected components
	    for(int i = 0;i<V;i++) {
	        if(color[i] == -1) {
	            if(dfs(i, 0, color, adj) == false) return false; 
	        }
	    }
	    return true; 
    }
     public static void main(String[] args)
    {
        // V = 4, E = 4
        ArrayList < ArrayList < Integer >> adj = new ArrayList < > ();
        for (int i = 0; i < 4; i++) {
            adj.add(new ArrayList < > ());
        }
        adj.get(0).add(2);
        adj.get(2).add(0);
        adj.get(0).add(3);
        adj.get(3).add(0);
        adj.get(1).add(3);
        adj.get(3).add(1);
        adj.get(2).add(3);
        adj.get(3).add(2);

        Solution obj = new Solution();
        boolean ans = obj.isBipartite(4, adj);
        if(ans)
            System.out.println("1");
        else System.out.println("0");
    }

}
```
## bfs implimentation
```java
import java.util.*;

public class BipartiteGraphAdjList {

    // Function to check if the graph is bipartite using BFS
    public static boolean isBipartite(List<List<Integer>> adjList, int vertices) {
        // Array to store colors of vertices: -1 means uncolored
        int[] colors = new int[vertices];
        Arrays.fill(colors, -1);

        // Iterate over all vertices to handle disconnected graphs
        for (int i = 0; i < vertices; i++) {
            if (colors[i] == -1) { // If the vertex is not yet colored
                if (!bfsCheck(adjList, i, colors)) {
                    return false;
                }
            }
        }
        return true;
    }

    private static boolean bfsCheck(List<List<Integer>> adjList, int start, int[] colors) {
        Queue<Integer> queue = new LinkedList<>();
        queue.add(start);
        colors[start] = 0; // Start coloring the first node as 0

        while (!queue.isEmpty()) {
            int node = queue.poll();
            for (int neighbor : adjList.get(node)) {
                if (colors[neighbor] == -1) { // If uncolored, assign alternate color
                    colors[neighbor] = 1 - colors[node];
                    queue.add(neighbor);
                } else if (colors[neighbor] == colors[node]) {
                    // If neighbor has the same color, it's not bipartite
                    return false;
                }
            }
        }
        return true;
    }

    public static void main(String[] args) {
        int vertices = 4;
        // Adjacency list representation of the graph
        List<List<Integer>> adjList = new ArrayList<>();
        for (int i = 0; i < vertices; i++) {
            adjList.add(new ArrayList<>());
        }

        // Adding edges
        adjList.get(0).add(1);
        adjList.get(0).add(3);
        adjList.get(1).add(0);
        adjList.get(1).add(2);
        adjList.get(2).add(1);
        adjList.get(2).add(3);
        adjList.get(3).add(0);
        adjList.get(3).add(2);

        if (isBipartite(adjList, vertices)) {
            System.out.println("The graph is bipartite.");
        } else {
            System.out.println("The graph is not bipartite.");
        }
    }
}
```
---
# Detect cycle in a directed graph (using DFS) 

The intuition is to reach a previously visited node again on the same path. If it can be done, we conclude that the graph has a cycle.

Note: If a directed graph contains a cycle, the node has to be visited again on the same path and not through different paths.
![image](https://github.com/soulnote/All-images/blob/main/cycleindg.gif)
```java
import java.util.*;


class Solution {
    private boolean dfsCheck(int node, ArrayList<ArrayList<Integer>> adj, int vis[], int pathVis[]) {
        vis[node] = 1; 
        pathVis[node] = 1; 
        
        // traverse for adjacent nodes 
        for(int it : adj.get(node)) {
            // when the node is not visited 
            if(vis[it] == 0) {
                if(dfsCheck(it, adj, vis, pathVis) == true) 
                    return true; 
            }
            // if the node has been previously visited
            // but it has to be visited on the same path 
            else if(pathVis[it] == 1) {
                return true; 
            }
        }
        
        pathVis[node] = 0; 
        return false; 
    }

    // Function to detect cycle in a directed graph.
    public boolean isCyclic(int V, ArrayList<ArrayList<Integer>> adj) {
        int vis[] = new int[V];
        int pathVis[] = new int[V];
        
        for(int i = 0;i<V;i++) {
            if(vis[i] == 0) {
                if(dfsCheck(i, adj, vis, pathVis) == true) return true; 
            }
        }
        return false; 
    }
}

public class tUf {
    public static void main(String[] args) {
        int V = 11;
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        adj.get(1).add(2);
        adj.get(2).add(3);
        adj.get(3).add(4);
        adj.get(3).add(7);
        adj.get(4).add(5);
        adj.get(5).add(6);
        adj.get(7).add(5);
        adj.get(8).add(9);
        adj.get(9).add(10);
        adj.get(10).add(8);

        Solution obj = new Solution();
        boolean ans = obj.isCyclic(V, adj);
        if (ans)
            System.out.println("True");
        else
            System.out.println("False");

    }
}
```
**Time Complexity:** O(V+E)+O(V) , where V = no. of nodes and E = no. of edges. There can be at most V components. So, another O(V) time complexity.

**Space Complexity:** O(2N) + O(N) ~ O(2N): O(2N) for two visited arrays and O(N) for recursive stack space.


# Topological Sort Algorithm | DFS
Problem Statement: Given a Directed Acyclic Graph (DAG) with V vertices and E edges, Find any Topological Sorting of that Graph.

Note: In topological sorting, node u will always appear before node v if there is a directed edge from node u towards node v(u -> v).

Example:
`Input: V = 6, E = 6`


![image](https://github.com/soulnote/LeetCode/assets/71943647/004e67dd-2379-4184-9319-8268155ed2c9)


Output: 5, 4, 2, 3, 1, 0
Explanation: A graph may have multiple topological sortings. 
The result is one of them. The necessary conditions 
for the ordering are:
According to edge 5 -> 0, node 5 must appear before node 0 in the ordering.
According to edge 4 -> 0, node 4 must appear before node 0 in the ordering.
According to edge 5 -> 2, node 5 must appear before node 2 in the ordering.
According to edge 2 -> 3, node 2 must appear before node 3 in the ordering.
According to edge 3 -> 1, node 3 must appear before node 1 in the ordering.
According to edge 4 -> 1, node 4 must appear before node 1 in the ordering.

The above result satisfies all the necessary conditions. 
[4, 5, 2, 3, 1, 0] is also one such topological sorting 
that satisfies all the conditions.

**Intuition:** Since we are inserting the nodes into the stack after the completion of the traversal, we are making sure, there will be no one who appears afterward but may come before in the ordering as everyone during the traversal would have been inserted into the stack. 
Note: Points to remember, that node will be marked as visited immediately after making the DFS call and before returning from the DFS call, the node will be pushed into the stack.
```java
import java.util.*;

class Solution {
    private static void dfs(int node, int vis[], Stack<Integer> st,
            ArrayList<ArrayList<Integer>> adj) {
        vis[node] = 1;
        for (int it : adj.get(node)) {
            if (vis[it] == 0)
                dfs(it, vis, st, adj);
        }
        st.push(node);
    }

    // Function to return list containing vertices in Topological order.
    static int[] topoSort(int V, ArrayList<ArrayList<Integer>> adj) {
        int vis[] = new int[V];
        Stack<Integer> st = new Stack<Integer>();
        for (int i = 0; i < V; i++) {
            if (vis[i] == 0) {
                dfs(i, vis, st, adj);
            }
        }

        int ans[] = new int[V];
        int i = 0;
        while (!st.isEmpty()) {
            ans[i++] = st.peek();
            st.pop();
        }
        return ans;
    }
}

public class tUf {
    public static void main(String[] args) {
        int V = 6;
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        adj.get(2).add(3);
        adj.get(3).add(1);
        adj.get(4).add(0);
        adj.get(4).add(1);
        adj.get(5).add(0);
        adj.get(5).add(2);

        int[] ans = Solution.topoSort(V, adj);
        for (int node : ans) {
            System.out.print(node + " ");
        }
        System.out.println("");
    }
}
```
# Kahn’s Algorithm | Topological Sort Algorithm | BFS
Problem Statement: Given a Directed Acyclic Graph (DAG) with V vertices and E edges, Find any Topological Sorting of that Graph.

Note: In topological sorting, node u will always appear before node v if there is a directed edge from node u towards node v(u -> v).

Example :

`Input Format: V = 6, E = 6`


![image](https://github.com/soulnote/LeetCode/assets/71943647/aafa2328-0f67-4b62-9240-38e92f30bd46)


Result: 5, 4, 0, 2, 3, 1
Explanation: A graph may have multiple topological sortings. 
The result is one of them. The necessary conditions for the ordering are:
According to edge 5 -> 0, node 5 must appear before node 0 in the ordering.
According to edge 4 -> 0, node 4 must appear before node 0 in the ordering.
According to edge 5 -> 2, node 5 must appear before node 2 in the ordering.
According to edge 2 -> 3, node 2 must appear before node 3 in the ordering.
According to edge 3 -> 1, node 3 must appear before node 1 in the ordering.
According to edge 4 -> 1, node 4 must appear before node 1 in the ordering.

The above result satisfies all the necessary conditions. 
[4, 5, 2, 3, 1, 0] and [4, 5, 0, 2, 3, 1] are also such 
topological sortings that satisfy all the conditions.


### 🔍 Intuition Summary for Kahn’s Algorithm (Cycle Detection using BFS):

We count how many edges come **into each node** (in-degree).
Then, we **start from nodes with no incoming edges** (in-degree 0) — they can be safely placed first.
As we remove a node from the queue, we **reduce the in-degree of its neighbors**, like "unlocking" the next steps.
If a neighbor's in-degree becomes 0, it means it's ready to be processed next, so we add it to the queue.
If we can't process all nodes this way, it means there's a **cycle blocking the order**.

```java
import java.util.*;

class Solution {
    // Function to return list containing vertices in Topological order.
    static int[] topoSort(int V, ArrayList<ArrayList<Integer>> adj) {
        int indegree[] = new int[V];
        for (int i = 0; i < V; i++) {
            for (int it : adj.get(i)) {
                indegree[it]++;
            }
        }

        Queue<Integer> q = new LinkedList<Integer>();
        ;
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.add(i);
            }
        }

        int topo[] = new int[V];
        int i = 0;
        while (!q.isEmpty()) {
            int node = q.peek();
            q.remove();
            topo[i++] = node;
            // node is in your topo sort
            // so please remove it from the indegree

            for (int it : adj.get(node)) {
                indegree[it]--;
                if (indegree[it] == 0) {
                    q.add(it);
                }
            }
        }

        return topo;
    }
}

public class tUf {
    public static void main(String[] args) {
        int V = 6;
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        adj.get(2).add(3);
        adj.get(3).add(1);
        adj.get(4).add(0);
        adj.get(4).add(1);
        adj.get(5).add(0);
        adj.get(5).add(2);

        int[] ans = Solution.topoSort(V, adj);
        for (int node : ans) {
            System.out.print(node + " ");
        }
        System.out.println("");
    }
}
```

# Detect Cycle in a Directed Graph using BFS

1. Use **Kahn’s Algorithm** to perform a BFS-based topological sort.
2. Calculate the **in-degree** of all vertices.
3. Add all vertices with **in-degree 0** to a queue.
4. Process each node from the queue and reduce in-degrees of neighbors.
5. If the total processed nodes are **less than V**, a **cycle exists**.

```java
// Java program to check if there is a cycle in 
// directed graph using BFS.
import java.io.*;
import java.util.*;

class GFG
{

    // Class to represent a graph
    static class Graph
    {
        int V; // No. of vertices'

        // Pointer to an array containing adjacency list
        Vector<Integer>[] adj;

        @SuppressWarnings("unchecked")
        Graph(int V)
        {
            // Constructor
            this.V = V;
            this.adj = new Vector[V];
            for (int i = 0; i < V; i++)
                adj[i] = new Vector<>();
        }

        // function to add an edge to graph
        void addEdge(int u, int v)
        {
            adj[u].add(v);
        }

        // Returns true if there is a cycle in the graph
        // else false.

        // This function returns true if there is a cycle
        // in directed graph, else returns false.
        boolean isCycle() 
        {

            // Create a vector to store indegrees of all
            // vertices. Initialize all indegrees as 0.
            int[] in_degree = new int[this.V];
            Arrays.fill(in_degree, 0);

            // Traverse adjacency lists to fill indegrees of
            // vertices. This step takes O(V+E) time
            for (int u = 0; u < V; u++)
            {
                for (int v : adj[u])
                    in_degree[v]++;
            }

            // Create an queue and enqueue all vertices with
            // indegree 0
            Queue<Integer> q = new LinkedList<Integer>();
            for (int i = 0; i < V; i++)
                if (in_degree[i] == 0)
                    q.add(i);

            // Initialize count of visited vertices
            int cnt = 0;

            // Create a vector to store result (A topological
            // ordering of the vertices)
            Vector<Integer> top_order = new Vector<>();

            // One by one dequeue vertices from queue and enqueue
            // adjacents if indegree of adjacent becomes 0
            while (!q.isEmpty())
            {

                // Extract front of queue (or perform dequeue)
                // and add it to topological order
                int u = q.poll();
                top_order.add(u);

                // Iterate through all its neighbouring nodes
                // of dequeued node u and decrease their in-degree
                // by 1
                for (int itr : adj[u])
                    if (--in_degree[itr] == 0)
                        q.add(itr);
                cnt++;
            }

            // Check if there was a cycle
            if (cnt != this.V)
                return true;
            else
                return false;
        }
    }

    // Driver Code
    public static void main(String[] args) 
    {

        // Create a graph given in the above diagram
        Graph g = new Graph(6);
        g.addEdge(0, 1);
        g.addEdge(1, 2);
        g.addEdge(2, 0);
        g.addEdge(3, 4);
        g.addEdge(4, 5);

        if (g.isCycle())
            System.out.println("Yes");
        else
            System.out.println("No");
    }
}

```
# Shortest Path in Undirected Graph with unit distance
Given an Undirected Graph having unit weight, find the shortest path from the source to all other nodes in this graph. In this problem statement, we have assumed the source vertex to be ‘0’. If a vertex is unreachable from the source node, then return -1 for that vertex.

Example 1:
Input:
n = 9, m = 10
edges = [[0,1],[0,3],[3,4],[4 ,5],[5, 6],[1,2],[2,6],[6,7],[7,8],[6,8]]
src=0 
![image](https://github.com/soulnote/LeetCode/assets/71943647/074fb182-0416-4a6e-afa6-43f47716218b)
Output: 0 1 2 1 2 3 3 4 4

Explanation:
The above output array shows the shortest path to all 
the nodes from the source vertex (0), Dist[0] = 0, 
Dist[1] = 1 , Dist[2] = 2 , …. Dist[8] = 4 
Where Dist[node] is the shortest path between src and 
the node. For a node, if the value of Dist[node]= -1, 
then we conclude that the node is unreachable from 
the src node.

**Intution:** We use BFS because it explores nodes level by level, making it ideal when all edge weights are 1 (unit weights).
Start from the source node and track distances in a distance array, initializing all distances to a large number (infinity).
As we move to adjacent nodes, we update (relax) their distance only if we found a shorter path through the current node.
Whenever a node's distance is updated, we push it into the queue to continue BFS from there.
By the end, the distance array gives the shortest path from the source to all reachable nodes; unreachable ones stay as -1.
```java
import java.util.*;
import java.lang.*;
import java.io.*;

class Main{
    
    public static void main(String[] args) throws IOException{
        int n=9, m=10;
        int[][] edge = {{0,1},{0,3},{3,4},{4,5},{5,6},{1,2},{2,6},{6,7},{7,8},{6,8}};
          
        Solution obj = new Solution();
        int res[] = obj.shortestPath(edge,n,m,0);
        for(int i=0;i<n;i++){
            System.out.print(res[i]+" ");
        }
        System.out.println();
    }
}

class Solution {
    
    public int[] shortestPath(int[][] edges,int n,int m ,int src) {
    //Create an adjacency list of size N for storing the undirected graph.
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>(); 
        for(int i = 0;i<n;i++) {
            adj.add(new ArrayList<>()); 
        }
        for(int i = 0;i<m;i++) {
            adj.get(edges[i][0]).add(edges[i][1]); 
            adj.get(edges[i][1]).add(edges[i][0]); 
        }
    //A dist array of size N initialised with a large number to 
    //indicate that initially all the nodes are untraversed. 
        int dist[] = new int[n];
        for(int i = 0;i<n;i++) dist[i] = (int)1e9;
        dist[src] = 0; 

    // BFS Implementation
        Queue<Integer> q = new LinkedList<>();
        q.add(src); 
        while(!q.isEmpty()) {
            int node = q.peek(); 
            q.remove(); 
            for(int it : adj.get(node)) {
                if(dist[node] + 1 < dist[it]) {
                    dist[it] = 1 + dist[node]; 
                    q.add(it); 
                }
            }
        }
        // Updated shortest distances are stored in the resultant array ‘ans’.
        // Unreachable nodes are marked as -1. 
        for(int i = 0;i<n;i++) {
            if(dist[i] == 1e9) {
                dist[i] = -1; 
            }
        }
        return dist; 
    }
}
```

# Shortest Path in Directed Acyclic Graph Topological Sort
Given a DAG, find the shortest path from the source to all other nodes in this DAG. In this problem statement, we have assumed the source vertex to be ‘0’. You will be given the weighted edges of the graph.

Note: What is a DAG ( Directed Acyclic Graph)?

A Directed Graph (containing one-sided edges) having no cycles is said to be a Directed Acyclic Graph.

Example :
![image](https://github.com/soulnote/LeetCode/assets/71943647/fcc12b35-a9b0-49e3-acb0-236be142f176)
Input: n = 6, m= 7
edges =[[0,1,2],[0,4,1],[4,5,4],[4,2,2],[1,2,3],[2,3,6],[5,3,1]]

Output: 0 2 3 6 1 5

Explanation:  The above output list shows the shortest path to all the nodes from the source vertex (0).

**Intution:** In a DAG, we can find shortest paths efficiently by processing nodes in topological order, so we always visit a node after all its dependencies.
Using DFS, we create this order and store it in a stack.
Starting from the source, we relax edges (update shortest distance) for each node using dist[u] + weight < dist[v].
Since there are no cycles, this ensures we never re-process a node unnecessarily.
The final dist[] array holds the shortest distance from the source to every node.

```java
import java.util.*;
import java.lang.*;
import java.io.*;

class Main {

  public static void main(String[] args) throws IOException {
    int n = 6, m = 7;
    int[][] edge = {{0,1,2},{0,4,1},{4,5,4},{4,2,2},{1,2,3},{2,3,6},{5,3,1}};
    Solution obj = new Solution();
    int res[] = obj.shortestPath(n, m, edge);
    for (int i = 0; i < n; i++) {
      System.out.print(res[i] + " ");
    }
    System.out.println();
  }
}

class Pair {
  int first, second;
  Pair(int _first, int _second) {
    this.first = _first;
    this.second = _second;
  }
}
//User function Template for Java
class Solution {
  private void topoSort(int node, ArrayList < ArrayList < Pair >> adj,
    int vis[], Stack < Integer > st) {
    //This is the function to implement Topological sort. 

    vis[node] = 1;
    for (int i = 0; i < adj.get(node).size(); i++) {
      int v = adj.get(node).get(i).first;
      if (vis[v] == 0) {
        topoSort(v, adj, vis, st);
      }
    }
    st.add(node);
  }
  public int[] shortestPath(int N, int M, int[][] edges) {
    ArrayList < ArrayList < Pair >> adj = new ArrayList < > ();
    for (int i = 0; i < N; i++) {
      ArrayList < Pair > temp = new ArrayList < Pair > ();
      adj.add(temp);
    }
    //We create a graph first in the form of an adjacency list.

    for (int i = 0; i < M; i++) {
      int u = edges[i][0];
      int v = edges[i][1];
      int wt = edges[i][2];
      adj.get(u).add(new Pair(v, wt));
    }
    int vis[] = new int[N];
    //Now, we perform topo sort using DFS technique 
    //and store the result in the stack st.

    Stack < Integer > st = new Stack < > ();
    for (int i = 0; i < N; i++) {
      if (vis[i] == 0) {
        topoSort(i, adj, vis, st);
      }
    }
    //Further, we declare a vector ‘dist’ in which we update the value of the nodes’
    //distance from the source vertex after relaxation of a particular node.
    int dist[] = new int[N];
    for (int i = 0; i < N; i++) {
      dist[i] = (int)(1e9);
    }

    dist[0] = 0;
    while (!st.isEmpty()) {
      int node = st.peek();
      st.pop();

      for (int i = 0; i < adj.get(node).size(); i++) {
        int v = adj.get(node).get(i).first;
        int wt = adj.get(node).get(i).second;

        if (dist[node] + wt < dist[v]) {
          dist[v] = wt + dist[node];
        }
      }
    }

    for (int i = 0; i < N; i++) {
      if (dist[i] == 1e9) dist[i] = -1;
    }
    return dist;
  }
}
```

# Dijkstra’s Algorithm – Using Priority Queue
Given a weighted, undirected, and connected graph of V vertices and an adjacency list adj where adj[i] is a list of lists containing two integers where the first integer of each list j denotes there is an edge between i and j, second integers corresponds to the weight of that edge. You are given the source vertex S and You have to Find the shortest distance of all the vertex from the source vertex S. You have to return a list of integers denoting the shortest distance between each node and the Source vertex S.

Note: The Graph doesn’t contain any negative weight cycle.
Examples: 
![image](https://github.com/soulnote/LeetCode/assets/71943647/153995ee-e20c-4811-98b7-4a7734c4aed4)

Input:

V = 2

adj [] = {{{1, 9}}, {{0, 9}}}

S = 0

Output:

0 9

Explanation: 
The source vertex is 0. Hence, the shortest distance of node 0 from the source is 0 and the shortest distance of node 1 from source will be 9.

**Intution 1 using set:** We want the shortest path from a source to all nodes, so we always explore the closest unvisited node first.
We use a Set to keep track of nodes sorted by current shortest distance, always processing the node with minimum distance first.
When visiting a node’s neighbors, if we find a better (shorter) path, we update the distance and push the new pair into the set.
The set ensures that we always deal with the most promising paths first and discard longer paths to the same node.
This approach works only with positive weights, as negative weights can lead to incorrect updates.
## **Intution 2 using queue:** 
We find the shortest path from a source node to all others using a min-heap priority queue to always process the node with the smallest distance first.
At each step, we pick the top node (lowest distance), then update its neighbors’ distances if a shorter path is found.
The updated neighbors are pushed back into the queue, and this process continues until all nodes are processed.
This way, we always prioritize the most optimal (shortest) paths.
Note: Works only for graphs with non-negative weights.

```java
import java.util.*;
import java.lang.*;
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {
         int V = 3, E=3,S=2;
    ArrayList<Integer> node1 = new ArrayList<Integer>() {{add(1);add(1);}};
    ArrayList<Integer> node2 = new ArrayList<Integer>() {{add(2);add(6);}};
    ArrayList<Integer> node3 = new ArrayList<Integer>() {{add(2);add(3);}};
    ArrayList<Integer> node4 = new ArrayList<Integer>() {{add(0);add(1);}};
    ArrayList<Integer> node5 = new ArrayList<Integer>() {{add(1);add(3);}};
    ArrayList<Integer> node6 = new ArrayList<Integer>() {{add(0);add(6);}};
    
    ArrayList<ArrayList<Integer>> inter1 = new ArrayList<ArrayList<Integer>>(){
      {
          add(node1);
          add(node2);
      }  
    };
    ArrayList<ArrayList<Integer>> inter2 = new ArrayList<ArrayList<Integer>>(){
      {
          add(node3);
          add(node4);
      }  
    };
    ArrayList<ArrayList<Integer>> inter3 = new ArrayList<ArrayList<Integer>>(){
      {
          add(node5);
          add(node6);
      }  
    };
    ArrayList<ArrayList<ArrayList<Integer>>> adj= new ArrayList<ArrayList<ArrayList<Integer>>>(){
        {
            add(inter1); // for 1st node
            add(inter2); // for 2nd node
            add(inter3); // for 3rd node
        }
    };
    //add final values of adj here.
    Solution obj = new Solution();
    int[] res= obj.dijkstra(V,adj,S);
    
    for(int i=0;i<V;i++){
        System.out.print(res[i]+" ");
    }
    System.out.println();
    
    }
}

class Pair{
    int node;
    int distance;
    public Pair(int distance,int node){
        this.node = node;
        this.distance = distance;
    }
}
//User function Template for Java
class Solution
{
    //Function to find the shortest distance of all the vertices
    //from the source vertex S.
    static int[] dijkstra(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj, int S)
    {
        // Create a priority queue for storing the nodes as a pair {dist, node
        // where dist is the distance from source to the node.  
        PriorityQueue<Pair> pq = 
        new PriorityQueue<Pair>((x,y) -> x.distance - y.distance);
        
        int []dist = new int[V]; 
      
        // Initialising distTo list with a large number to
        // indicate the nodes are unvisited initially.
        // This list contains distance from source to the nodes.
        for(int i = 0;i<V;i++) dist[i] = (int)(1e9); 
        
        // Source initialised with dist=0.
        dist[S] = 0;
        pq.add(new Pair(0,S)); 
        
        // Now, pop the minimum distance node first from the min-heap
        // and traverse for all its adjacent nodes.
        while(pq.size() != 0) {
            int dis = pq.peek().distance; 
            int node = pq.peek().node; 
            pq.remove(); 
            
            // Check for all adjacent nodes of the popped out
            // element whether the prev dist is larger than current or not.
            for(int i = 0;i<adj.get(node).size();i++) {
                int edgeWeight = adj.get(node).get(i).get(1); 
                int adjNode = adj.get(node).get(i).get(0); 
                
                // If current distance is smaller,
                // push it into the queue.
                if(dis + edgeWeight < dist[adjNode]) {
                    dist[adjNode] = dis + edgeWeight; 
                    pq.add(new Pair(dist[adjNode], adjNode)); 
                }
            }
        }
        // Return the list containing shortest distances
        // from source to all the nodes.
        return dist; 
    }
}
```
# Bellman Ford Algorithm
Problem Statement: Given a weighted, directed and connected graph of V vertices and E edges, Find the shortest distance of all the vertices from the source vertex S.

Note: If the Graph contains a negative cycle then return an array consisting of only -1.

Example:
Input Format: 
V = 6, 
E = [[3, 2, 6], [5, 3, 1], [0, 1, 5], [1, 5, -3], [1, 2, -2], [3, 4, -2], [2, 4, 3]], 
S = 0
![image](https://github.com/soulnote/LeetCode/assets/71943647/a523bcfc-fcc2-46ea-a0a7-1b8a00b5533a)

Result: 0 5 3 3 1 2
Explanation: Shortest distance of all nodes from the source node is returned.

### 🔍 Intuition for Bellman-Ford Algorithm:

Bellman-Ford helps find the **shortest path from a source node** even when the graph has **negative edge weights**, which Dijkstra cannot handle.
The key idea is to **relax all edges** repeatedly — in each of the **V-1 iterations** (V = number of vertices), we try to improve the shortest distances.
After these iterations, we **check once more**: if any edge can still be relaxed, that means the graph contains a **negative cycle**, and we return `[-1]`.

In short:

* Works for **negative weights**.
* Can **detect negative cycles**.
* Time complexity: **O(V × E)**, slower than Dijkstra but more powerful in certain cases.

```java
import java.util.*;

/*
*   edges: vector of vectors which represents the graph
*   S: source vertex to start traversing graph with
*   V: number of vertices
*/
class Solution {
    static int[] bellman_ford(int V,
                              ArrayList<ArrayList<Integer>> edges, int S) {
        int[] dist = new int[V];
        for (int i = 0; i < V; i++) dist[i] = (int)(1e8);
        dist[S] = 0;
        // V x E
        for (int i = 0; i < V - 1; i++) {
            for (ArrayList<Integer> it : edges) {
                int u = it.get(0);
                int v = it.get(1);
                int wt = it.get(2);
                if (dist[u] != 1e8 && dist[u] + wt < dist[v]) {
                    dist[v] = dist[u] + wt;
                }
            }
        }
        // Nth relaxation to check negative cycle
        for (ArrayList<Integer> it : edges) {
            int u = it.get(0);
            int v = it.get(1);
            int wt = it.get(2);
            if (dist[u] != 1e8 && dist[u] + wt < dist[v]) {
                int temp[] = new int[1];
                temp[0] = -1;
                return temp;
            }
        }
        return dist;
    }
}

public class tUf {
    public static void main(String[] args) {
        int V = 6;
        int S = 0;
        ArrayList<ArrayList<Integer>> edges = new ArrayList<>() {
            {
                add(new ArrayList<Integer>(Arrays.asList(3, 2, 6)));
                add(new ArrayList<Integer>(Arrays.asList(5, 3, 1)));
                add(new ArrayList<Integer>(Arrays.asList(0, 1, 5)));
                add(new ArrayList<Integer>(Arrays.asList(1, 5, -3)));
                add(new ArrayList<Integer>(Arrays.asList(1, 2, -2)));
                add(new ArrayList<Integer>(Arrays.asList(3, 4, -2)));
                add(new ArrayList<Integer>(Arrays.asList(2, 4, 3)));
            }
        };



        int[] dist = Solution.bellman_ford(V, edges, S);
        for (int i = 0; i < V; i++) {
            System.out.print(dist[i] + " ");
        }
        System.out.println("");
    }
}
```
Output: 0 5 3 3 1 2

Time Complexity: O(V*E), where V = no. of vertices and E = no. of Edges.
Space Complexity: O(V) for the distance array which stores the minimized distances.

# Floyd Warshall Algorithm
Problem Statement: The problem is to find the shortest distances between every pair of vertices in a given edge-weighted directed graph. The graph is represented as an adjacency matrix of size n*n. Matrix[i][j] denotes the weight of the edge from i to j. If Matrix[i][j]=-1, it means there is no edge from i to j.

Do it in place.

Example 1:

Input Format: 
matrix[][] = { {0, 2, -1, -1},{1, 0, 3, -1},{-1, -1, 0, -1},{3, 5, 4, 0} }

Result:
0 2 5 -1 
1 0 3 -1 
-1 -1 0 -1 
3 5 4 0 
Explanation: In this example, the final matrix 
is storing the shortest distances. For example, matrix[i][j] is 
storing the shortest distance from node i to j.

Sure! Here’s a **concise summary** combining the approach, steps, and intuition of the Floyd-Warshall algorithm:

---

### Floyd-Warshall Algorithm — Approach, Steps & Intuition

**Approach:**
Use dynamic programming on an adjacency matrix to find the shortest distances between every pair of nodes by considering each node as a possible intermediate point.

**Steps:**

1. Initialize the adjacency matrix with direct edge weights; use infinity where no direct edge exists. Set distance to self as 0.
2. For each node `k` (intermediate), update all pairs `(i, j)` by checking if going through `k` is shorter:
   `matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j])`
3. After processing all nodes as intermediate, `matrix[i][j]` holds the shortest distance from `i` to `j`.
4. (Optional) Check diagonal elements for negative values to detect negative cycles.

**Intuition:**
By iteratively allowing paths to pass through each node, Floyd-Warshall finds the shortest route between every pair of vertices, considering all possible intermediate nodes.

```java
import java.util.*;


//User function template for JAVA

class Solution {
    public void shortest_distance(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == -1) {
                    matrix[i][j] = (int)(1e9);
                }
                if (i == j) matrix[i][j] = 0;
            }
        }

        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    matrix[i][j] = Math.min(matrix[i][j],
                                            matrix[i][k] + matrix[k][j]);
                }
            }
        }

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == (int)(1e9)) {
                    matrix[i][j] = -1;
                }
            }
        }
    }
}

public class tUf {
    public static void main(String[] args) {
        int V = 4;
        int[][] matrix = new int[V][V];

        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                matrix[i][j] = -1;
            }
        }

        matrix[0][1] = 2;
        matrix[1][0] = 1;
        matrix[1][2] = 3;
        matrix[3][0] = 3;
        matrix[3][1] = 5;
        matrix[3][2] = 4;

        Solution obj = new Solution();
        obj.shortest_distance(matrix);

        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println("");
        }
    }
} 
```
# Minimum Spanning Tree
In this article, we will be discussing the minimum spanning tree. So, to understand the minimum spanning tree, we first need to discuss what a spanning tree is.

Let’s further discuss this below:

Spanning Tree:
A spanning tree is a tree in which we have N nodes(i.e. All the nodes present in the original graph) and N-1 edges and all nodes are reachable from each other.

Let’s understand this using an example. Assume we are given an undirected weighted graph with N nodes and M edges. Here in this example, we have taken N as 5 and M as 6.


![image](https://github.com/soulnote/LeetCode/assets/71943647/05e8518f-4d00-403d-8d6c-afa26cb47ce9)

For the above graph, if we try to draw a spanning tree, the following illustration will be one:

![image](https://github.com/soulnote/LeetCode/assets/71943647/29490407-b9cd-496e-b60a-add276239a4c)

We can draw more spanning trees for the given graph. Two of them are like the following:

![image](https://github.com/soulnote/LeetCode/assets/71943647/81d2c2cc-cb62-4e2f-b38f-e457646d4028)

Note: Point to remember is that a graph may have more than one spanning trees.

All the above spanning trees contain some edge weights. For each of them, if we add the edge weights we can get the sum for that particular tree. Now, let’s try to figure out the minimum spanning tree:

Minimum Spanning Tree:
Among all possible spanning trees of a graph, the minimum spanning tree is the one for which the sum of all the edge weights is the minimum.
Let’s understand the definition using the given graph drawn above. Until now, for the given graph we have drawn three spanning trees with the sum of edge weights 18, 24, and 18. If we can draw all possible spanning trees, we will find that the following spanning tree with the minimum sum of edge weights 16 is the minimum spanning tree for the given graph:

![image](https://github.com/soulnote/LeetCode/assets/71943647/5c978133-907c-4247-8852-78a41ce55faf)

Note: There may exist multiple minimum spanning trees for a graph like a graph may have multiple spanning trees.

# Prim’s Algorithm – Minimum Spanning Tree

Problem Statement: Given a weighted, undirected, and connected graph of V vertices and E edges. The task is to find the sum of weights of the edges of the Minimum Spanning Tree.
(Sometimes it may be asked to find the MST as well, where in the MST the edge-informations will be stored in the form {u, v}(u = starting node, v = ending node).)

Example 1:

Input Format: 
V = 5, edges = { {0, 1, 2}, {0, 3, 6}, {1, 2, 3}, {1, 3, 8}, {1, 4, 5}, {4, 2, 7}}


![image](https://github.com/soulnote/LeetCode/assets/71943647/8f449a09-65c7-4138-b9ff-428cb9fed777)


Result: 16
Explanation: 
The minimum spanning tree for the given graph is drawn below:
MST = {(0, 1), (0, 3), (1, 2), (1, 4)}

![image](https://github.com/soulnote/LeetCode/assets/71943647/e3df31a1-96e6-4ba6-b7c5-8f78f688a024)

```java
import java.util.*;

// User function Template for Java

class Pair {
    int node;
    int distance;
    public Pair(int distance, int node) {
        this.node = node;
        this.distance = distance;
    }
}
class Solution {
    //Function to find sum of weights of edges of the Minimum Spanning Tree.
    static int spanningTree(int V,
                            ArrayList<ArrayList<ArrayList<Integer>>> adj) {
        PriorityQueue<Pair> pq =
            new PriorityQueue<Pair>((x, y) -> x.distance - y.distance);

        int[] vis = new int[V];
        // {wt, node}
        pq.add(new Pair(0, 0));
        int sum = 0;
        while (pq.size() > 0) {
            int wt = pq.peek().distance;
            int node = pq.peek().node;
            pq.remove();

            if (vis[node] == 1) continue;
            // add it to the mst
            vis[node] = 1;
            sum += wt;

            for (int i = 0; i < adj.get(node).size(); i++) {
                int edW = adj.get(node).get(i).get(1);
                int adjNode = adj.get(node).get(i).get(0);
                if (vis[adjNode] == 0) {
                    pq.add(new Pair(edW, adjNode));
                }
            }
        }
        return sum;
    }
}

public class tUf {
    public static void main(String[] args) {
        int V = 5;
        ArrayList<ArrayList<ArrayList<Integer>>> adj = new ArrayList<ArrayList<ArrayList<Integer>>>();
        int[][] edges =  {{0, 1, 2}, {0, 2, 1}, {1, 2, 1}, {2, 3, 2}, {3, 4, 1}, {4, 2, 2}};

        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<ArrayList<Integer>>());
        }

        for (int i = 0; i < 6; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int w = edges[i][2];

            ArrayList<Integer> tmp1 = new ArrayList<Integer>();
            ArrayList<Integer> tmp2 = new ArrayList<Integer>();
            tmp1.add(v);
            tmp1.add(w);

            tmp2.add(u);
            tmp2.add(w);

            adj.get(u).add(tmp1);
            adj.get(v).add(tmp2);
        }

        Solution obj = new Solution();
        int sum = obj.spanningTree(V, adj);
        System.out.println("The sum of all the edge weights: " + sum);
    }
}
```

# Disjoint Set | Union by Rank | Union by Size | Path Compression
In this article, we will discuss the Disjoint Set data structure which is a very important topic in the entire graph series. Let’s first understand why we need a Disjoint Set data structure using the below question:

Question: Given two components of an undirected graph

![image](https://github.com/soulnote/LeetCode/assets/71943647/c6bd9bad-e89c-4f7f-8470-3ec5c9189ac7)

The question is whether node 1 and node 5 are in the same component or not.
```java
import java.io.*;
import java.util.*;
class DisjointSet {
    List<Integer> rank = new ArrayList<>();
    List<Integer> parent = new ArrayList<>();
    public DisjointSet(int n) {
        for (int i = 0; i <= n; i++) {
            rank.add(0);
            parent.add(i);
        }
    }

    public int findUPar(int node) {
        if (node == parent.get(node)) {
            return node;
        }
        int ulp = findUPar(parent.get(node));
        parent.set(node, ulp);
        return parent.get(node);
    }

    public void unionByRank(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return;
        if (rank.get(ulp_u) < rank.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);
        } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
            parent.set(ulp_v, ulp_u);
        } else {
            parent.set(ulp_v, ulp_u);
            int rankU = rank.get(ulp_u);
            rank.set(ulp_u, rankU + 1);
        }
    }

}

class Main {
    public static void main (String[] args) {
        DisjointSet ds = new DisjointSet(7);
        ds.unionByRank(1, 2);
        ds.unionByRank(2, 3);
        ds.unionByRank(4, 5);
        ds.unionByRank(6, 7);
        ds.unionByRank(5, 6);

        // if 3 and 7 same or not
        if (ds.findUPar(3) == ds.findUPar(7)) {
            System.out.println("Same");
        } else
            System.out.println("Not Same");

        ds.unionByRank(3, 7);
        if (ds.findUPar(3) == ds.findUPar(7)) {
            System.out.println("Same");
        } else
            System.out.println("Not Same");
    }
}
```
# Kruskal’s Algorithm – Minimum Spanning Tree 
Problem Statement: Given a weighted, undirected, and connected graph of V vertices and E edges. The task is to find the sum of weights of the edges of the Minimum Spanning Tree.

Example :

Input Format: 
V = 5, edges = { {0, 1, 2}, {0, 3, 6}, {1, 2, 3}, {1, 3, 8}, {1, 4, 5}, {4, 2, 7}}


![image](https://github.com/soulnote/LeetCode/assets/71943647/b3a4f11d-e5ee-4b5f-bd01-d21be3e93837)

Result: 16
Explanation: The minimum spanning tree for the given graph is drawn below:
MST = {(0, 1), (0, 3), (1, 2), (1, 4)}

```java
import java.io.*;
import java.util.*;


// User function Template for Java

class DisjointSet {
    List<Integer> rank = new ArrayList<>();
    List<Integer> parent = new ArrayList<>();
    List<Integer> size = new ArrayList<>();
    public DisjointSet(int n) {
        for (int i = 0; i <= n; i++) {
            rank.add(0);
            parent.add(i);
            size.add(1);
        }
    }

    public int findUPar(int node) {
        if (node == parent.get(node)) {
            return node;
        }
        int ulp = findUPar(parent.get(node));
        parent.set(node, ulp);
        return parent.get(node);
    }

    public void unionByRank(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return;
        if (rank.get(ulp_u) < rank.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);
        } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
            parent.set(ulp_v, ulp_u);
        } else {
            parent.set(ulp_v, ulp_u);
            int rankU = rank.get(ulp_u);
            rank.set(ulp_u, rankU + 1);
        }
    }

    public void unionBySize(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return;
        if (size.get(ulp_u) < size.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);
            size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));
        } else {
            parent.set(ulp_v, ulp_u);
            size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));
        }
    }
}
class Edge implements Comparable<Edge> {
    int src, dest, weight;
    Edge(int _src, int _dest, int _wt) {
        this.src = _src; this.dest = _dest; this.weight = _wt;
    }
    // Comparator function used for
    // sorting edgesbased on their weight
    public int compareTo(Edge compareEdge) {
        return this.weight - compareEdge.weight;
    }
};
class Solution {
    //Function to find sum of weights of edges of the Minimum Spanning Tree.
    static int spanningTree(int V,
                            ArrayList<ArrayList<ArrayList<Integer>>> adj) {
        List<Edge> edges = new ArrayList<Edge>();
        // O(N + E)
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < adj.get(i).size(); j++) {
                int adjNode = adj.get(i).get(j).get(0);
                int wt = adj.get(i).get(j).get(1);
                int node = i;
                Edge temp = new Edge(i, adjNode, wt);
                edges.add(temp);
            }
        }
        DisjointSet ds = new DisjointSet(V);
        // M log M
        Collections.sort(edges);
        int mstWt = 0;
        // M x 4 x alpha x 2
        for (int i = 0; i < edges.size(); i++) {
            int wt = edges.get(i).weight;
            int u = edges.get(i).src;
            int v = edges.get(i).dest;

            if (ds.findUPar(u) != ds.findUPar(v)) {
                mstWt += wt;
                ds.unionBySize(u, v);
            }
        }

        return mstWt;
    }
}

class Main {
    public static void main (String[] args) {
        int V = 5;
        ArrayList<ArrayList<ArrayList<Integer>>> adj = new ArrayList<ArrayList<ArrayList<Integer>>>();
        int[][] edges =  {{0, 1, 2}, {0, 2, 1}, {1, 2, 1}, {2, 3, 2}, {3, 4, 1}, {4, 2, 2}};

        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<ArrayList<Integer>>());
        }

        for (int i = 0; i < 6; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int w = edges[i][2];

            ArrayList<Integer> tmp1 = new ArrayList<Integer>();
            ArrayList<Integer> tmp2 = new ArrayList<Integer>();
            tmp1.add(v);
            tmp1.add(w);

            tmp2.add(u);
            tmp2.add(w);

            adj.get(u).add(tmp1);
            adj.get(v).add(tmp2);
        }

        Solution obj = new Solution();
        int mstWt = obj.spanningTree(V, adj);
        System.out.println("The sum of all the edge weights: " + mstWt);

    }
}
```
# Bridges in Graph – Using Tarjan’s Algorithm of time in and low time

Problem Statement: There are n servers numbered from 0 to n – 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. Any server can reach other servers directly or indirectly through the network.

A critical connection is a connection that, if removed, will make some servers unable to reach some other servers.

Return all critical connections in the network in any order.

Note: Here servers mean the nodes of the graph. The problem statement is taken from leetcode.

Pre-requisite: DFS algorithm

Example :

Input Format: N = 4, connections = [[0,1],[1,2],[2,0],[1,3]]


![image](https://github.com/soulnote/LeetCode/assets/71943647/77dd1c18-d5b7-4daa-924b-33673742e2c6)

Result: [[1, 3]]
Explanation: The edge [1, 3] is the critical edge because if we remove the edge the graph will be divided into 2 components.

```java
import java.io.*;
import java.util.*;

class Solution {
    private int timer = 1;
    private void dfs(int node, int parent, int[] vis,
                     ArrayList<ArrayList<Integer>> adj, int tin[], int low[],
                     List<List<Integer>> bridges) {
        vis[node] = 1;
        tin[node] = low[node] = timer;
        timer++;
        for (Integer it : adj.get(node)) {
            if (it == parent) continue;
            if (vis[it] == 0) {
                dfs(it, node, vis, adj, tin, low, bridges);
                low[node] = Math.min(low[node], low[it]);
                // node --- it
                if (low[it] > tin[node]) {
                    bridges.add(Arrays.asList(it, node));
                }
            } else {
                low[node] = Math.min(low[node], low[it]);
            }
        }
    }
    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
        ArrayList<ArrayList<Integer>> adj =
            new ArrayList<ArrayList<Integer>>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<Integer>());
        }
        for (List<Integer> it : connections) {
            int u = it.get(0); int v = it.get(1);
            adj.get(u).add(v);
            adj.get(v).add(u);
        }
        int[] vis = new int[n];
        int[] tin = new int[n];
        int[] low = new int[n];
        List<List<Integer>> bridges = new ArrayList<>();
        dfs(0, -1, vis, adj, tin, low, bridges);
        return bridges;
    }
}

class Main {
    public static void main (String[] args) {
        int n = 4;
        int[][] edges = {
            {0, 1}, {1, 2},
            {2, 0}, {1, 3}
        };
        List<List<Integer>> connections = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            connections.add(new ArrayList<Integer>());
        }
        for (int i = 0; i < n; i++) {
            connections.get(i).add(edges[i][0]);
            connections.get(i).add(edges[i][1]);
        }

        Solution obj = new Solution();
        List<List<Integer>> bridges = obj.criticalConnections(n, connections);

        int size = bridges.size();
        for (int i = 0; i < size; i++) {
            int u = bridges.get(i).get(0);
            int v = bridges.get(i).get(1);
            System.out.print("[" + u + ", " + v + "] ");
        }
        System.out.println("");
    }
} 
```

# Articulation Point in Graph
Problem Statement: Given an undirected connected graph with V vertices and adjacency list adj. You are required to find all the vertices removing which (and edges through it) disconnect the graph into 2 or more components.

Note: Indexing is zero-based i.e nodes numbering from (0 to V-1). There might be loops present in the graph.

Pre-requisite: Bridges in Graph problem & DFS algorithm.

Example :

Input Format:


![image](https://github.com/soulnote/LeetCode/assets/71943647/287a11a5-6100-4eb8-9664-6648b8a412fe)

Result: {0, 2}
Explanation: If we remove node 0 or node 2, the graph will be divided into 2 or more components.


![image](https://github.com/soulnote/LeetCode/assets/71943647/8374512c-4109-47c3-a1ef-c4c4ecfe1435)

```java
import java.io.*;
import java.util.*;



class Solution {
    private int timer = 1;
    private void dfs(int node, int parent, int[] vis,
                     int tin[], int low[], int[] mark,
                     ArrayList<ArrayList<Integer>> adj) {
        vis[node] = 1;
        tin[node] = low[node] = timer;
        timer++;
        int child = 0;
        for (Integer it : adj.get(node)) {
            if (it == parent) continue;
            if (vis[it] == 0) {
                dfs(it, node, vis, tin, low, mark, adj);
                low[node] = Math.min(low[node], low[it]);
                // node --- it
                if (low[it] >= tin[node] && parent != -1) {
                    mark[node] = 1;
                }
                child++;
            } else {
                low[node] = Math.min(low[node], tin[it]);
            }
        }
        if (child > 1 && parent == -1) {
            mark[node] = 1;
        }
    }
    //Function to return Breadth First Traversal of given graph.
    public ArrayList<Integer> articulationPoints(int n,
            ArrayList<ArrayList<Integer>> adj) {
        int[] vis = new int[n];
        int[] tin = new int[n];
        int[] low = new int[n];
        int[] mark = new int[n];
        for (int i = 0; i < n; i++) {
            if (vis[i] == 0) {
                dfs(i, -1, vis, tin, low, mark, adj);
            }
        }
        ArrayList<Integer> ans = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (mark[i] == 1) {
                ans.add(i);
            }
        }
        if (ans.size() == 0) {
            ans.add(-1);
        }
        return ans;
    }
}

class Main {
    public static void main (String[] args) {
        int n = 5;
        int[][] edges = {
            {0, 1}, {1, 4},
            {2, 4}, {2, 3}, {3, 4}
        };
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<Integer>());
        }
        for (int i = 0; i < n; i++) {
            int u = edges[i][0], v = edges[i][1];
            adj.get(u).add(v);
            adj.get(v).add(u);
        }

        Solution obj = new Solution();
        ArrayList<Integer> nodes = obj.articulationPoints(n, adj);

        int size = nodes.size();
        for (int i = 0; i < size; i++) {
            int node = nodes.get(i);
            System.out.print(node + " ");
        }
        System.out.println("");
    }
}
```
# Strongly Connected Components – Kosaraju’s Algorithm

Problem Statement: Given a Directed Graph with V vertices (Numbered from 0 to V-1) and E edges, Find the number of strongly connected components in the graph.

Pre-requisite: DFS algorithm

Example :

Input Format:


![image](https://github.com/soulnote/LeetCode/assets/71943647/6760c3e1-9256-4e90-b6e5-098cf0e0b1f7)

Result: 3
Explanation: Three strongly connected components are marked below:


![image](https://github.com/soulnote/LeetCode/assets/71943647/5427953b-59c3-49f8-bb61-24c9f0d9951f)

A **strongly connected component (SCC)** in a **directed graph** is a maximal subgraph in which:

1. **Every vertex is reachable from every other vertex** in that subgraph.
2. **Maximal** means that no additional vertices can be added to the subgraph while maintaining the property of strong connectivity.

In simple terms, for every pair of vertices \(u\) and \(v\) in an SCC:
- There exists a directed path from \(u\) to \(v\), and
- There exists a directed path from \(v\) to \(u\).

### Example:
Consider the following directed graph:

```
0 → 1 → 2 → 0
3 → 4
```

Here:
1. The vertices **0, 1, 2** form an SCC because there are paths between every pair of these vertices in both directions.
2. The vertex **3** forms its own SCC because there is no way to reach other vertices bidirectionally.
3. The vertex **4** also forms its own SCC for the same reason.

### Why are SCCs important?
- **Applications in Graph Theory**: SCCs help in analyzing strongly connected substructures within a larger graph.
- **Real-World Use Cases**:
  - Dependency analysis in software modules.
  - Finding cycles in directed graphs.
  - Network analysis, such as identifying clusters or communities in social networks.

### Kosaraju's Algorithm for Finding SCCs:
The most common algorithm for finding SCCs (like the one shared above) includes:
1. **DFS to determine the order of processing (finish times)**.
2. **Reverse the graph**.
3. **Perform DFS in the reverse graph in the order of the finish times** to find all SCCs.
Here is the Java implementation to find the number of strongly connected components (SCCs) in a directed graph using **Kosaraju's algorithm**:

### Explanation of Kosaraju's Algorithm:
1. Perform a **DFS** on the original graph and store the vertices in a stack according to their finishing times.
2. Reverse the graph (transpose the edges).
3. Perform a **DFS** on the reversed graph, considering vertices in the order defined by the stack.
4. Each DFS traversal in the reversed graph marks one SCC.

### Java Implementation:

```java
import java.util.*;

public class KosarajuAlgorithm {

    // Function to find the number of strongly connected components
    public static int findSCCs(int V, List<List<Integer>> adj) {
        // Step 1: Perform DFS on the original graph and store vertices by finish time
        Stack<Integer> stack = new Stack<>();
        boolean[] visited = new boolean[V];
        
        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                dfsOriginal(i, adj, visited, stack);
            }
        }

        // Step 2: Reverse the graph
        List<List<Integer>> reversedGraph = reverseGraph(V, adj);

        // Step 3: Perform DFS on the reversed graph using vertices in stack order
        Arrays.fill(visited, false); // Reset the visited array
        int sccCount = 0;

        while (!stack.isEmpty()) {
            int node = stack.pop();
            if (!visited[node]) {
                dfsReversed(node, reversedGraph, visited);
                sccCount++; // Each DFS on the reversed graph identifies one SCC
            }
        }

        return sccCount;
    }

    private static void dfsOriginal(int node, List<List<Integer>> adj, boolean[] visited, Stack<Integer> stack) {
        visited[node] = true;
        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                dfsOriginal(neighbor, adj, visited, stack);
            }
        }
        stack.push(node); // Push to stack after all neighbors are visited
    }

    private static void dfsReversed(int node, List<List<Integer>> reversedGraph, boolean[] visited) {
        visited[node] = true;
        for (int neighbor : reversedGraph.get(node)) {
            if (!visited[neighbor]) {
                dfsReversed(neighbor, reversedGraph, visited);
            }
        }
    }

    private static List<List<Integer>> reverseGraph(int V, List<List<Integer>> adj) {
        List<List<Integer>> reversedGraph = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            reversedGraph.add(new ArrayList<>());
        }

        for (int i = 0; i < V; i++) {
            for (int neighbor : adj.get(i)) {
                reversedGraph.get(neighbor).add(i); // Reverse the edge
            }
        }

        return reversedGraph;
    }

    public static void main(String[] args) {
        int V = 5; // Number of vertices
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }

        // Adding edges
        adj.get(0).add(2);
        adj.get(2).add(1);
        adj.get(1).add(0);
        adj.get(0).add(3);
        adj.get(3).add(4);

        // Find and print the number of SCCs
        System.out.println("The number of strongly connected components is: " + findSCCs(V, adj));
    }
}
```

### Input:
The graph is represented as an adjacency list.  
Vertices: `0, 1, 2, 3, 4`  
Edges:  
- 0 → 2  
- 2 → 1  
- 1 → 0  
- 0 → 3  
- 3 → 4  

### Output:
```
The number of strongly connected components is: 3
```

### Explanation:
1. SCCs in this graph are:
   - {0, 1, 2}  
   - {3}  
   - {4}  

This implementation efficiently computes the SCCs in \(O(V + E)\) time complexity.
