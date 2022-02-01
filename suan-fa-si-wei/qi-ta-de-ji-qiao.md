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

###
