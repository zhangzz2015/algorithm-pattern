# Google VO

### 1月6号VO，3轮coding，1轮BQ

有一些commits，按照performance 非递增排序，但是performance是隐藏的，只能通过调用isWorse来比较其中两个commit的performance，要求找到比上个commit差的commit（一开始我给了一个个比较的方法，然后就应该是用binary search找commit，code写完了，但是没时间跑test case了）

#### [Campus Bikes](https://leetcode.com/problems/campus-bikes/)

```cpp
// Some code
// Time complexity O(n*m*log(n*m)) + O(n*m)  
// Space O(n*m)   
// Base on,  sort all pair bike and worker. 
class Solution {
public:
    vector<int> assignBikes(vector<vector<int>>& workers, vector<vector<int>>& bikes) {
        
        
        vector<pair<int, pair<int,int>>>  record; // distance  iwork  jbike
        for(int i=0; i< workers.size(); i++){
            for(int j=0; j< bikes.size(); j++){
                int distance = abs(workers[i][0]-bikes[j][0]) + abs(workers[i][1]-bikes[j][1]); 
                record.push_back(make_pair(distance, make_pair(i, j))); 
            }
        }
        
        sort(record.begin(), record.end(), [](pair<int, pair<int,int>>& node1, pair<int, pair<int,int>>& node2)
             {
                 if(node1.first == node2.first)
                 {
                     if(node1.second.first == node2.second.first)
                         return node1.second.second < node2.second.second;
                     return node1.second.first < node2.second.first; 
                 }
                 return node1.first < node2.first;                  
             }
            );
        vector<int> use_workers(workers.size(), -1); 
        vector<bool> use_bikes(bikes.size(), false); 
        for(int i=0; i< record.size(); i++ )
        {
            auto iworker = record[i].second.first; 
            auto ibike = record[i].second.second; 
            if(use_workers[iworker]!=-1 || 
               use_bikes[ibike]!=false)
                 continue; 
            
            use_workers[iworker] = ibike; 
            use_bikes[ibike] = true;             
        }
            
        return use_workers; 
        
    }
};
```

####

