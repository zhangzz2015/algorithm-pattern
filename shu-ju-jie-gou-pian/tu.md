# 图

### 图的基本知识

1. 图有有向图和无向图
2. 图的表征有连接表和连接矩阵
3. 图的遍历有BFS和DFS方法

### 图的常用算法

```
// Some code
BFS 方法
vector<vector<int>> graph; 

for(int i=0; i< n; i++)
{

}
```

### 图的二分

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

DFS方法使用着色方法，需要辅助visit flag。需要三个状态，0为没有访问，1为正在访问，2为访问结束，该方法也可用来判断有向图或无向图环的问题。注意建立图的时候DFS方法和BFS方法对于有向图正好相反。

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

