# 其他的技巧

### 操作矩阵

[48. Rotate Image](https://leetcode.com/problems/rotate-image/)

把矩阵旋转转化为矩阵的转置和对称操作。

90 rotate = transpose + mirror column.  180 rotate = mirror column + mirror row.  270 rotate = transpose + mirror row.

```cpp
// Some code
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        
        rotate(matrix, 90);         
    }
    void rotate(vector<vector<int>>& matrix, int degree)
    {
       switch(degree)
       {
             case 90:
               transpose(matrix);
               mirror(matrix, false); 
               break; 
             case 180:
               mirror(matrix, false); 
               mirror(matrix, true); 
               break; 
             case 270:
               transpose(matrix); 
               mirror(matrix, true); 
               break; 
             default: 
               cout<< "Don't support rotate value:%d" << degree << "\n";
               break; 
       }                
    }
    
    // transpose matrix. 
    void transpose(vector<vector<int>>& matrix)
    {
        // [i j] --> [j, i]
        int n = matrix.size(); 
        for(int i=0; i< n; i++)
        {
            for(int j=i+1; j< n; j++)
            {
                swap(matrix[i][j], matrix[j][i]); 
            }
        }
    }
    void mirror(vector<vector<int>>& matrix, bool rowOrCol)
    {
        // [i, j]  --- > [i, n-1-j]
        int n = matrix.size(); 
        if(rowOrCol==false) // mirror col
        {
            for(int i=0; i< n; i++)
            {
                for(int j=0; j<n/2; j++)
                {
                    swap(matrix[i][j], matrix[i][n-1-j]); 
                }
            }
        }
        else // mirror row
        {
            for(int i=0; i< n; i++)
            {
                for(int j=0; j<n/2; j++)
                {
                    swap(matrix[j][i], matrix[n-1-j][i]); 
                }
            }            
        }
        
    }
};
```

[48. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

记录四个边界点，left right top bot。总过有四个方向操作，第一个方向为从左到右，第二个方向从上到下，第三个方向为从右到左，第四个方向从下到上。

```cpp
// Some code
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        
        int left =0; 
        int right = matrix[0].size()-1; 
        int top = 0;
        int down = matrix.size()-1; 
        
        vector<int> ret; 
        int direction = 0; // 0 from left to right 1 from top to down, 2 from right to left, 3 from down to top. 
        while(left<=right && top <= down)
        {
            if(direction ==0)
            {
               for(int i=left; i<=right; i++)
                   ret.push_back(matrix[top][i]); 
                top++; 
                direction =1; 
            }
            else if(direction ==1)
            {
                for(int i=top; i<=down; i++)
                    ret.push_back(matrix[i][right]); 
                right--; 
                direction = 2; 
                
            }
            else if(direction ==2)
            {
                for(int i=right; i>=left; i--)
                    ret.push_back(matrix[down][i]); 
                
                down--; 
                direction = 3; 
            }
            else
            {
               for(int i=down; i>=top; i--)
                   ret.push_back(matrix[i][left]); 
                left++; 
                direction = 0; 
            }                        
        }        
        return ret;                      
    }
};
```

[59 Spiral Matrix II ](https://leetcode.com/problems/spiral-matrix-ii/)

使用相同的方法和Spiral Matrix.&#x20;

```cpp
// Some code
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        
        vector<vector<int>> ret(n, vector<int>(n)); 
        
        int left = 0; 
        int right = n-1; 
        int top = 0; 
        int down = n-1; 
        int direction =0; 
        int count =0; 
        while(left<=right && top <= down)
        {
            if(direction==0)
            {
                for(int i=left ; i<=right; i++)
                {
                    count++;                                         
                    ret[top][i] = count; 
                }
                direction++; 
                top++; 
            }
            else if(direction ==1)
            {
                for(int i = top ; i<=down; i++)
                {
                    count++;                                         
                    ret[i][right] = count; 
                }
                direction ++; 
                right--;             
            }
            else if(direction ==2)
            {
                for(int i= right; i>=left; i--)
                {
                    count++; 
                    ret[down][i] = count; 
                }
                direction++; 
                down--; 
            }
            else if(direction==3)
            {
                for(int i= down; i>=top ; i--)
                {
                    count++; 
                    ret[i][left] = count; 
                }
                direction =0; 
                left++; 
            }                        
        }
        
        return ret; 
        
    }
};
```

### Trie

```cpp
// Some code
struct triNode
{
vector<triNode*> child;
bool isWord;   
   triNode()
   {
     child.assign(26,NULL); 
     isWord= false; 
   }
}; 

class Trie {
public:
    triNode* root; 
    Trie() {
      root = new triNode();   
    }
    
    void insert(string word) {
       triNode* current =root; 
       for(int i=0; i< word.size(); i++)
       {
          int index =word[i] - 'a'; 
          if(current->child[index]==NULL)
          {
             current->child[index] = new triNode(); 
          }
          current = current->child[index]; 
       }
       current->isWord= true;         
    }
    
    bool search(string word) {
       triNode* current =root; 
       for(int i=0; i< word.size(); i++)
       {
          int index = word[i] - 'a'; 
           if(current->child[index]==NULL)
              return false; 
            current = current->child[index];    
       }
       
       return current ->isWord;         
    }
    
    bool startsWith(string prefix) {
        triNode* current = root; 
        for(int i=0; i< prefix.size(); i++)
        {
           int index = prefix[i] - 'a'; 
           if(current->child[index]==NULL)
              return false; 
           current = current->child[index]; 
        }
        return true; 
    }
};

```

#### [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

```cpp
// Some code++
/// use trie. 
struct Trie
{
    vector<Trie*> child; 
    bool isWord; 
    Trie()
    {
        child.resize(26, NULL); 
        isWord = false; 
    }        
}; 
class WordDictionary {
public:
    Trie * root; 
    WordDictionary() {
        
        root = new Trie(); 
    }
    
    void addWord(string word) {
        
        Trie * current = root; 
        for(int i =0; i< word.size(); i++)
        {
            int index = word[i]-'a';
            if(current->child[index]==NULL)
            {
                current->child[index] = new Trie; 
            }
            current = current->child[index]; 
        }
        current->isWord = true; 
        
    }
    
    bool search(string word) {
        
        return dfs(word, 0, root); 
                        
    }
    
    bool dfs(string& word, int start, Trie* root)
    {
        if(start == word.size())
        {
           if(root)
               return root->isWord; 
           else
               return false; 
        }
        
        if(word[start]=='.')
        {
            for(int i=0; i< root->child.size(); i++){                
                if(root->child[i])
                {
                    if(dfs(word, start+1, root->child[i]))
                        return true; 
                }
            }
        }
        else 
        {
            int index= word[start] -'a'; 
            if(root->child[index])
            {
                if(dfs(word, start+1, root->child[index]))
                    return true; 
            }
        }
        
        return false; 
        
    }
};
```

### [Majority Element II](https://leetcode.com/problems/majority-element-ii)

Boyer-Moore Majority Vote algorithm：利用最多数的特点，如果超过1/k的比列则表明最多有k-1个数。当我们达到了k-1，则检查是否在candidate中，如果是则+1，否则则把所有candidate -1，抵消一次。如果不足k-1，则首先检查是否在candiate中，如果是则+1，否则把当前数放入candidate，并计数为一次。

```
// Some code
// 
    vector<int> major(vector<int>& nums, int k)
    {
        unordered_map<int, int> record; 
        
        for(int i=0; i< nums.size(); i++)
        {
            if(record.size()==k-1 )
            {
               if(record.count(nums[i])) 
                  record[nums[i]]++;
               else
               {
                   auto it = record.begin();
                   while(it!=record.end())
                   {
                       (*it).second--; 
                       if((*it).second==0)
                        {
                            it= record.erase(it); 
                        }
                       else
                           it++; 
                   }
               }  
            }
            else if(record.size()<(k-1) )
            {
               if(record.count(nums[i])) 
                  record[nums[i]]++;                
               else  
                  record[nums[i]]=1; 
            }            
        }
        for(auto it = record.begin(); it!=record.end(); it++)
        {
            (*it).second=0; 
        }
        for(int i=0; i< nums.size(); i++)
        {
            if(record.count(nums[i]))
            {
                record[nums[i]]++; 
            }
        }
        vector<int> ret; 
        for(auto it = record.begin(); it!=record.end(); it++)
        {
            if((*it).second>nums.size()/k)
            {
                ret.push_back((*it).first); 
            }
        }
        return ret; 
        
    }
    
    vector<int> major2(vector<int>& nums)
    {
        int count1=0;
        int count2=0;
        int nums1 =0; 
        int nums2 =0; 
        for(int i=0; i< nums.size(); i++)
        {
            if(count1>0 && nums1 == nums[i])
            {
                count1++; 
            }
            else if(count2>0 && nums2 == nums[i])
            {
                count2++; 
            }
            else if(count1 == 0)
            {
                nums1 = nums[i]; 
                count1++; 
            }
            else if(count2 ==0)
            {
                nums2 = nums[i];
                count2++; 
            }
            else // for other situation. 
            {
                count1--; 
                count2--;
            }                        
        }
        
        int sum1=0; 
        int sum2=0;
        for(int i=0; i< nums.size(); i++)
        {
            if(count1>0 && nums[i] == nums1)
            {
                sum1++; 
            }
            else if(count2>=0 && nums[i] == nums2)
            {
                sum2++; 
            }
        }
        vector<int> ret; 
        if(sum1> nums.size()/3 )
            ret.push_back(nums1); 
        if(sum2> nums.size()/3 )
            ret.push_back(nums2);   
        return ret;         
    }
```

###
