# 图

### 图的基本知识

1. 图有有向图和无向图
2. 图的表征有连接表和连接矩阵
3. 图的遍历有BFS和DFS方法

### 图的常用算法

#### BFS 模板

```
// BFS method 1.   Time complexity O(V+E)  Space O(V)
void bfs(vector<vector<int>>& graph, int source)
{
    vector<bool> visit(graph.size(),false); 
    queue<int> que; 
    que.push(source); 
    visit[source] = true; 
    while(que.size()){
       int topNode = que.front(); 
        que.pop(); 
        for(int i=0; i< graph[topNode].size(); i++){
            int nextNode = graph[topNode][j]; 
           if(visit[nextNode]==false) { // avoid loop. 
              que.push(nextNode); 
           }
        } 
    }    
}

// BFS method 2.   Time complexity O(V+E)  Space O(V)
void bfs(vector<vector<int>>& graph, int source)
{
    vector<bool> visit(graph.size(),false); 
    queue<int> que; 
    que.push(source); 
    while(que.size()){
       int topNode = que.front(); 
        que.pop(); 
        if(visit[topNode]) continue;  // avoid loop and repeating 
        visit[topNode] = true; 
        for(int i=0; i< graph[topNode].size(); i++){
            int nextNode = graph[topNode][j]; 
              que.push(nextNode); 
           }
        } 
    }    
}
```

#### DFS 模板

```cpp
// method 1. 
//Starting from a given node, traverse all the reachable nodes using 
//dpth first heuristic and each node is only visited once.
// Mark the node as visited when first time entering the node.
// time: O(V + E) every node will be visited once O(V) and 
// every edge will only be generated once.
//  space: O(V)
void dfs(vector<vector<int>>& graph, int root, vector<bool>& visit)
{
   // node was visited only once. Some edge may not visit. 
   if(visit[root]) return;  
   visit[root] = true; 
   
   for(int i=0; i< graph[root].size(); i++)
   {
       int nextNode = graph[root][i]; 
       dfs(graph, nextNode, visit);  
   }

}
// method 2. 
// Mark visited after all its descendants.
// We need additional state "visiting" to deal with a special
// marker of the nodes on the current dfs path to avoid duplicately visiting nodes.
/// Mark the nodes as visitin when first time entering the node, 
///Mark the node as visited when all the neighbors are visited.

time: O(E + V)
space: O(V)
// visit  0  mean not visit. 
//        1  mean visiting. 
//        2  visited. 
void dfs(vector<vector<int>>& graph, int root, vector<int>& visit)
{
   // node was visited only once. Some edge may not visit. 
   if(visit[root]==2) return;   // visited. don't visit. 
   if(visit[root]==1) return;   // visiting. means has loop. 
   visit[root] = 1; 
   for(int i=0; i< graph[root].size(); i++)
   {
       int nextNode = graph[root][i]; 
       dfs(graph, nextNode, visit);  
   }
   visit[root] = 2 //   finsh all descendants
}

// method 3.   
// Care about the pathes, instead of the vertex is reachable in this case.
// Find all paths from the start node, on the path there is no duplicate vertices.
// only mark for the nodes on the current dfs path, avoiding cycle on the dfs path

//time: O(branch^V)
// space: O(V)
void dfs(vector<vector<int>>& graph, int root, vector<int>& visit)
{
   // node was visited only once. Some edge may not visit. 
   if(visit[root]==1) return;   // visiting. means has loop. 
   visit[root] = 1; 
   for(int i=0; i< graph[root].size(); i++)
   {
       int nextNode = graph[root][i]; 
       dfs(graph, nextNode, visit);  
   }
   visit[root] = 0 //   reset it. You may visit same node mulitple times. 
}
```

#### Union find 模板

```
// Some code
class UF
{
private:
    vector<int> parent; 
    vector<int> weight; 
    int totalNum; 
public:     
    
    UF(int n)
    {
       weight.assign(n, 0);
       totalNum =n; 
        for(int i=0; i< n; i++)
        {
            parent.push_back(i);
        }
    }
    
    int findParent(int p)
    {
        while(parent[p]!=p)
        {
           parent[p] = parent[parent[p]]; 
           p = parent[p];
        }
        return p; 
    }
    
    void unionTwoNode(int p, int q)
    {
        int parentP = findParent(p);
        int parentQ = findParent(q); 
        if(parentP==parentQ)
            return; 
        if(weight[parentP]>weight[parentQ])
        {
            parent[parentQ] = parentP; 
            weight[parentP]+=weight[parentQ]; 
        }
        else
        {
            parent[parentP] = parentQ; 
            weight[parentQ]+=weight[parentP]; 
        }
        totalNum--; 
    }
};
```

#### Topology order模板

#### floydWarshall方法

可用来计算任意两点的最短距离。时间复杂度为O(n^3)，也可处理负边的情况。

```cpp
// Some code
////  use dp to calculate all node min distance. 
//  o[n^3]    space  o[n^2] 
void floydWarshall(int N, vector<vector<int>>& connections){
    vector<vector<int>> dp;  // graph matrix.  dp[i][j]  between node i and node j distance.
    for (const auto& conn : connections) {
        edges.emplace_back(conn[0], conn[1], conn[2]);
        dp[conn[0]][conn[2]] = conn[3]
        // es.emplace_back(conn[1], conn[0], conn[2]);  // 如果是无向图，加上反向边
    }
    vector<vector<int>> d(N, vector<int>(N, INT_MAX));  // 两节点间最短距离
    for (int i = 0; i < N; ++i) d[i][i] = 0;
    for (int k = 0; k < N; ++k) { /// middle node. see if i to k and k to j dis is small. 
       for (int i = 0; i < N; ++i){
           for (int j = 0; j < N; ++j) {
               d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
           }
       }
    }
}++

```

#### dijkstra方法

利用贪心方法计算从source 到任意target的最短路径。不能处理负权重。

```cpp
// Some code
void dijkstra(int start, int N, vector<vector<int>>& connections) {
    vector<vector<pair<int, int>>> graph(N, vector<pair<int, int>>());
    // pair <current shortest path dis，node number>  possible shortest path. 
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq; 
    for (const auto& conn : connections) {
        graph[conn[0]].push_back(make_pair(conn[2], conn[1]));
        // graph[conn[1]].push_back(make_pair(conn[2], conn[0]));  // if undiction graph, add revers edge.
    }
    vector<int> d(N, INT_MAX);  // real shortest path from target to i node.
    d[start] = 0; // first. set start shortest path as zero. 
    pq.push(make_pair(0, start)); // and push into queue. 
    while (!pq.empty()) {
        pair<int, int> nextShortest = pq.top();
        pq.pop();
        if (d[nextShortest.second] < nextShortest.first) continue; // We already find shortest node. skip. Don't need to set visit flag.
                                           // otherwise, we loop neighbour node. and upgrade queue 
        for (const auto & v: graph[from]) { 
            auto [cost, to] = v;
            if (d[to] > d[from] + cost) {
                d[to] = d[from] + cost;
                pq.push(make_pair(d[to], to));
            }
        }
    }
}
```



### BFS方法

#### 752. Open the Lock

> 思路：转化该问题为从source到target找到最短的路径。使用BFS方法，注意visit node避免重复，另外避开deadend。

```cpp
// Some code
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        
        unordered_set<string> deadrecord; 
        for(int i=0; i< deadends.size(); i++)
            deadrecord.insert(deadends[i]); 
        unordered_set<string> visit; 
        return bfs(deadrecord, "0000", target, visit); 
    }
    
    int bfs(unordered_set<string> deadrecord,  string start, string target,  unordered_set<string>& visit)
    {
        
        queue<string> que; 
        que.push(start); 
        int level =0; 
        while(que.size())
        {
            int iSize = que.size(); 
            for(int i=0; i< iSize; i++){
                string topNode = que.front(); 
                que.pop(); 
                if(topNode == target)
                    return level; 
                for(int j=0; j< topNode.size(); j++)
                {
                    string tmpNode = topNode; 
                    if(tmpNode[j]=='9')
                        tmpNode[j] = '0'; 
                    else
                        tmpNode[j]++; 
                    if(visit.count(tmpNode)==0 && deadrecord.count(topNode)==0)
                    {
                        que.push(tmpNode); 
                        visit.insert(tmpNode); 
                    }
                    tmpNode = topNode; 
                    if(tmpNode[j] == '0')
                        tmpNode[j] = '9';
                    else
                        tmpNode[j]--; 
                    if(visit.count(tmpNode)==0&&deadrecord.count(topNode)==0)
                    {
                        que.push(tmpNode);                                                                 
                        visit.insert(tmpNode); 
                    }
                }
            }         
            level++; 
        }        
        return -1;                 
    }
        
};
```

### 图的二分

使用涂色方法，相邻的边涂不同的颜色，对于起始可随机选择任何颜色，如果碰见冲突则表明无法完成二分，可使用BFS或DFS。

#### [886. Possible Bipartition](https://leetcode.com/problems/possible-bipartition/)

```
// Some code
class Solution {
public:
    bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
        
        vector<int> color(n, -1);
        
        vector<vector<int>> graph(n); 
        // build graph
        for(int i=0; i< dislikes.size(); i++ )
        {
            graph[dislikes[i][0]-1].push_back(dislikes[i][1]-1); 
            graph[dislikes[i][1]-1].push_back(dislikes[i][0]-1);             
        }
        for(int i=0; i< n; i++ )
        {
            if(color[i]==-1)
            {
                color[i] = 1;                 
                if(dfs(graph, i, color)==false)
                    return false; 
//                if(bfs(graph, i, color)==false)
//                    return false; 
            }
        }
        return true; 
        
    }    
    bool bfs(vector<vector<int>>& graph, int iroot, vector<int>& color)
    {
        
        queue<int> que; 
        que.push(iroot); 
        while(que.size())
        {
            int isize= que.size(); 
            for(int i=0; i< isize; i++)
            {
                int topNode = que.front(); 
                que.pop(); 
                int topNodeColor = color[topNode]; 
                for(int i=0; i< graph[topNode].size(); i++)
                {
                    int nextNode = graph[topNode][i]; 
                    if(color[nextNode] == -1)
                    {
                        color[nextNode] = !(topNodeColor); 
                        que.push(nextNode); 
                    }
                    else if(color[nextNode] == topNodeColor)
                    {
                        return false; 
                    }                    
                }                
            }            
        }
        return true;                 
    }
    
    bool dfs(vector<vector<int>>& graph, int iroot, vector<int>& color)
    {
        int top_node_color = color[iroot]; 
        for(int i=0; i< graph[iroot].size(); i++)
        {
            int next_node = graph[iroot][i];
            if(color[next_node]==-1)
            {
                color[next_node] = !(top_node_color); 
                if(dfs(graph, next_node, color)==false)
                    return false; 
            }
            else if(color[next_node] == top_node_color)
                return false;             
        }
        
        return true; 
    }
};
```

### 拓扑排序

基本方法有BFS和DFS

BFS使用入度方法，先放入入度为零的点放入队列，遍历这些点的边，并减少边的另外的点的入度，如果入度减少到零则放入队列，最后检查是否把所有的点放入队列，不用考虑环的情况。

```cpp
// Some code
vector<int> topoOrder(vector<vector<int>>& graph, vector<int>& inorder)
{
    int node = graph.size(); 
    queue<int> que; 
    // put all inorder node into que. 
    for(int i=0; i< node; i++){
        if(inorder[i]==0){
            que.push(i);
        }
    }
    vector<int> res; 
    while(que.size()){
        int isize =que.size(); 
        
        for(int i=0; i< que.size(); i++)
        {
            int topNode = que.front(); 
            que.pop(); 
            res.push_back(topNode); 
            for(int j=0; j < graph[topNode].size(); j++){
                int nextNode =graph[topNode][j]; 
                inorder[nextNode]--; 
                if(inorder[nextNode]==0){
                    que.push(nextNode); 
                }
            }
        }
    }
    
    if(res.size() == graph.size())
       return res; 
    else 
        return {}; 
} 

```

DFS方法使用着色方法，需要辅助visit flag。需要三个状态，0为没有访问，1为正在访问，2为访问结束，该方法也可用来判断有向图或无向图环的问题。注意建立图的时候DFS方法和BFS方法对于有向图正好相反，否则DFS返回需要reverse处理。

```cpp
// Some code
vector<int> topoOrder(vector<vector<int>>& graph)
{
    // visit 0 unvisit 1 visiting 2 visited. 
    int n = graph.size(); 
    vector<char> visit(n, 0);
    vector<int> ret; 
    for(int i=0; i< n; i++ ){
        if(visit[i]==0){
            if(dfs(graph, i, visit, ret)==false)
                return {}; 
        }            
    }            
    return ret; 
} 

bool dfs(vector<vector<int>>& graph, int rootNode, vector<char>& visit,  vector<int>& ret)
{
    if(visit[rootNode]==1)
        return false; 
    
    if(visit[rootNode]==2)
        return true; 
    
    visit[rootNode] = 1; 
    for(int i=0; i< graph[rootNode].size(); i++)
    {
        if(dfs(graph, graph[rootNode][i], visit, ret)==false)
            return false;
    }        
    ret.push_back(rootNode);
    visit[rootNode] = 2; 
    return true;         
}
```

#### [1203 Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)

> 使用top 排序，但是复杂在于这里有两层top排序，一层为group，一层为item。同时group因为有一些item没有分配group，需要把没有分配group分配一个新的group id。建立Group 和item 图后进行top 排序。之后再争对输出要求进行分组。

```cpp
// Some code
class Solution {
public:
    vector<int> sortItems(int n, int m, vector<int>& group, vector<vector<int>>& beforeItems) {
        
        for(int i=0; i< group.size(); i++)
        {
            if(group[i]==-1)
            {
                group[i]=m;
                m++;                 
            }
        }

        
        vector<vector<int>> graphGroup(m);
        vector<int> inorderGroup(m,0); 
        vector<vector<int>> graphItem(n);
        vector<int> inorderItem(n,0); 
        
        for(int i=0; i<group.size(); i++)
        {
                for(int j =0; j < beforeItems[i].size(); j++)
                {
                    if(group[beforeItems[i][j]] != group[i])
                    {
                        graphGroup[group[i]].push_back(group[beforeItems[i][j]]); 
                    }
                    graphItem[i].push_back(beforeItems[i][j]);
                }
        }
        
        vector<int> groupOrder= topoOrder(graphGroup); 
        if(groupOrder.size()==0)
            return vector<int>();
        vector<int> itemOrder = topoOrder(graphItem); 
        if(itemOrder.size()==0)
            return vector<int>();
        vector<vector<int>> groupSort(m); 
        for(int i=0; i< itemOrder.size(); i++)
        {
            groupSort[group[itemOrder[i]]].push_back(itemOrder[i]); 
        }
        vector<int> ret;
        for(int i=0; i< groupOrder.size(); i++)
        {
            for(int j=0; j< groupSort[groupOrder[i]].size(); j++)
            {
                ret.push_back(groupSort[groupOrder[i]][j]); 
            }
        }
        return ret; 
        
     }      
    
    vector<int> topoOrder(vector<vector<int>>& graph)
    {
        // visit 0 unvisit 1 visiting 2 visited. 
        int n = graph.size(); 
         vector<char> visit(n, 0);
        vector<int> ret; 
        for(int i=0; i< n; i++ )
        {
            if(visit[i]==0)
            {
                if(dfs(graph, i, visit, ret)==false)
                    return {}; 
            }            
        }            
        return ret; 
    } 
    
    bool dfs(vector<vector<int>>& graph, int rootNode, vector<char>& visit,  vector<int>& ret)
    {
        if(visit[rootNode]==1)
            return false; 
        
        if(visit[rootNode]==2)
            return true; 
        
        visit[rootNode] = 1; 
        for(int i=0; i< graph[rootNode].size(); i++)
        {
            if(dfs(graph, graph[rootNode][i], visit, ret)==false)
                return false;
        }        
        ret.push_back(rootNode);
        visit[rootNode] = 2; 
        return true;         
    }
    
    
};
```
