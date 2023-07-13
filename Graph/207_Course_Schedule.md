# Intuition
<!-- Describe your first thoughts on how to solve this problem. -->
It is clear by reading problem statement that one course (ai) is dependent on other course (bi). so we can think of it like directed graph in which in order to get to course ai we first have to complete or visit bi course. 

So we can construct a `graph with edges bi -> ai`. Now in this graph if there is a cycle present then we can say that courses have circular dependency on each other and we can not complete all course.

Now, this problem narrows down to whether the graph is containing cycle or not which can be easily done via DFS/BFS. 
# Approach
<!-- Describe your approach to solving the problem. -->
As per above explanation we need to check if there is a `cycle present or not`.

In dfs what we can do is we start dfs for each node if it is not visited and during DFS if you encounter any node that is visited already and it is not the parent of current node than cycle is present in the graph.
 
Here I have used BFS solution in which I do `topological sorting` of the graph. And if in topo sort all the nodes are covered than we can say that all the courses can be completed. Topological sorting solution works because in topo sort we remove nodes that have `indegree == 0` one by one or level by level. Indegree == 0 states that course is not dependent on any other course so it can be completed.

In this way by maintaing queue we can find whether all courses can be completed or not.

# Complexity
- Time complexity: O(N)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: O(N)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```
class Solution {
    public boolean canFinish(int n, int[][] arr) {

        int [] indegree = new int [n];

        // graph
        List<List<Integer>> adj = new ArrayList<>();

        for(int i=0; i<n; i++) adj.add(new ArrayList<>());

        for(int [] ar : arr){
            indegree [ar[0]]++;
            adj.get(ar[1]).add(ar[0]); 
            //edge we have to add is bi -> ai  (so indeg[ai]++)
        }

        Queue<Integer> q = new LinkedList<>();
        for(int i=0; i<n; i++){
            if(indegree[i] == 0) q.offer(i);
        }

        int nodes = 0;
        while(!q.isEmpty()) {
            int cur = q.poll();
            nodes++;

            //remove edges that contains current course as it is completed
            for(int num : adj.get(cur)) {
                indegree[num]--;
                if(indegree[num] == 0) q.offer(num);
            }
        }

        //check if all nodes are visited or not
        return nodes == n;
    }
}
```

Thanks for reading the solution. Upvote if you are able to understand the problem solution.