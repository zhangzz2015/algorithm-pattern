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
