# Intuition
<!-- Describe your first thoughts on how to solve this problem. -->
In this problem you have to find array containing all the `safe nodes` of the graph. And as per given in the statement safe node is a node if every possible path starting from that node leads to a terminal node (or another safe node).

Here if you observe carefully in graph any path will lead either to a terminal node or node get back to node itself. In other words any path in the graph will either end at `leaf node` or will `contain cycle`.

So in this problem the nodes which are not safe nodes are those who are residing in the cycle or any path of that node is leading to a cycle. Remaining nodes are our answers.

# Approach
<!-- Describe your approach to solving the problem. -->
Here we will do simple dfs for each node to check whether the node is safe node or not. 

In which we will maintain one array `visited`. In which 0 will state that node is visited previously, 1 states that node is visited but in any previous dfs calls not in current path, 2 states that node is visited in current dfs call.

During dfs if you find that node is already visited than we will check if the `visited[node] == 1`. if it is one than the current node does not contain cycle and if `visited[node] == 2` than we can say that the node is visited previously in the same path so it has cycle.

This problem can also be solved using Topological sorting with the same time and space complexity.

# Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(n + n) extra n for recursion stack space
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```
class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        
        if(graph == null || graph.length == 0)  return new ArrayList<Integer>();

        int [] visited = new int [graph.length];
        List<Integer> res = new ArrayList<>(); 

        //check for cycle
        for(int i=0; i<visited.length; i++){
            if(dfs(i, graph, visited)) res.add(i);
        }

        return res;
    }

    //function to check if cycle exists
    public boolean dfs(int curr, int [][] graph, int [] visited){
        
        if(visited[curr] != 0){
            return visited[curr] == 1;
        }

        //2 indicates that current node is visited in this path
        visited[curr] = 2;

        for(int num : graph[curr]){
            if(!dfs(num, graph, visited)) return false;
        }

        // 1 indicates that node is not visited in current path (in previous DFS)
        visited[curr] = 1;

        return true;
    }
}
```