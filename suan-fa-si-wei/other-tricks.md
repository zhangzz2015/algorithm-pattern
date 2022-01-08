# Other tricks

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
