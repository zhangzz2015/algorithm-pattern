# 回溯法

## 背景

回溯法（backtrack）常用于遍历列表所有子集，是 DFS 深度搜索一种，一般用于全排列，穷尽所有可能，遍历的过程实际上是一个决策树的遍历过程。时间复杂度一般 O(N!)，它不像动态规划存在重叠子问题可以优化，回溯算法就是纯暴力穷举，复杂度一般都很高。

## 模板

```go
result = []
func backtrack(选择列表,路径):
    if 满足结束条件:
        result.add(路径)
        return
    for 选择 in 选择列表:
        做选择
        backtrack(选择列表,路径)
        撤销选择
```

核心就是从选择列表里做一个选择，然后一直递归往下搜索答案，如果遇到路径不通，就返回来撤销这次选择。

## 示例

### [subsets](https://leetcode-cn.com/problems/subsets/)

> 给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

遍历过程

![image.png](https://img.fuiboom.com/img/backtrack.png)

```cpp

Use backTracking method. Draw recursion tree. Every tree node means one solution. 
It is a little differenet with combination. But main logic is similar.
1 2 3
/ \
2 3 3
/
3
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        
        vector<int> onePath; 
        vector<vector<int>> ret; 
        backTrac(nums, 0, onePath, ret); 
        
        return ret; 
        
    }
    
    void backTrac(vector<int>& nums, int start, vector<int>& onePath, vector<vector<int>>& ret)
    {
        ret.push_back(onePath);         
        for(int i=start; i< nums.size(); i++ )
        {
            onePath.push_back(nums[i]); 
            backTrac(nums, i+1, onePath, ret); 
            onePath.pop_back(); 
            
        }
        
    }
};
```

### [subsets-ii](https://leetcode-cn.com/problems/subsets-ii/)

> 给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。说明：解集不能包含重复的子集。

```go
import (
	"sort"
)

func subsetsWithDup(nums []int) [][]int {
	// 保存最终结果
	result := make([][]int, 0)
	// 保存中间结果
	list := make([]int, 0)
	// 先排序
	sort.Ints(nums)
	backtrack(nums, 0, list, &result)
	return result
}

// nums 给定的集合
// pos 下次添加到集合中的元素位置索引
// list 临时结果集合(每次需要复制保存)
// result 最终结果
func backtrack(nums []int, pos int, list []int, result *[][]int) {
	// 把临时结果复制出来保存到最终结果
	ans := make([]int, len(list))
	copy(ans, list)
	*result = append(*result, ans)
	// 选择时需要剪枝、处理、撤销选择
	for i := pos; i < len(nums); i++ {
        // 排序之后，如果再遇到重复元素，则不选择此元素
		if i != pos && nums[i] == nums[i-1] {
			continue
		}
		list = append(list, nums[i])
		backtrack(nums, i+1, list, result)
		list = list[0 : len(list)-1]
	}
}
```

### [Combinations](https://leetcode.com/problems/combinations/)

```cpp
// Some code
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        
        //   1  2  3  ... 
        //  /   /
        // 2..n 3..n  
        
        vector<int> onePath; 
        vector<vector<int>> ret; 
        
        backTrac(n, k, 1, onePath, ret);
        return ret; 
        
    }
    
    void backTrac(int n, int k, int start, vector<int>& onePath, vector<vector<int>>& ret)
    {
        if(onePath.size()==k)
        {
            ret.push_back(onePath); 
            return; 
        }
        
        for(int i=start; i<=n; i++)
        {
            onePath.push_back(i); 
            backTrac(n, k, i+1, onePath, ret); 
            onePath.pop_back();             
        }        
    }
};c++

```

### [permutations](https://leetcode-cn.com/problems/permutations/)

> 给定一个   没有重复   数字的序列，返回其所有可能的全排列。

思路：需要记录已经选择过的元素，满足条件的结果才进行返回

```cpp
// method 1.  
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        int n = nums.size(); 
        vector<bool> visit(n, false); 
        vector<int> onePath; 
        vector<vector<int>> ret; 
        
        backTrac(nums, visit, onePath, ret); 
        return ret; 
    }
    
    void backTrac(vector<int>& nums, vector<bool>& visit,  vector<int>& onePath, vector<vector<int>>& ret)
    {
        if(onePath.size()== nums.size())
        {
            ret.push_back(onePath);             
            return ; 
        }
                
        for(int i=0; i< nums.size(); i++)
        {
            if(visit[i]==false)
            {
                onePath.push_back(nums[i]); 
                visit[i] = true; 
                backTrac(nums, visit, onePath, ret);             
                onePath.pop_back();
                visit[i] = false; 
            }            
        }                
    }
};
// method 2.
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        //   convert to full tree. And use visit flag as upper level is used. 
        ///       1     2   3 
        ///      /\    /  \   
        //   1 2  3   1[2]3
        //     /   \  /  \ 
        //    3    2  3 [2]  1  
        vector<vector<int>> ret;         
        backTrac(nums, ret, 0 ); 
        return ret; 
    }
    
    
    void backTrac(vector<int>& nums,  vector<vector<int>>& ret, int start)
    {
        if(start>=nums.size())
        {        
          ret.push_back(nums); 
        }
        for(int i=start; i< nums.size(); i++)
        {
            swap(nums[start], nums[i]); 
            backTrac(nums, ret, start+1); 
            swap(nums[start], nums[i]);             
        }                
    }        
}; 
```

### [permutations-ii](https://leetcode-cn.com/problems/permutations-ii/)

> 给定一个可包含重复数字的序列，返回所有不重复的全排列。注意剪枝策略，包括去重复

```go
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) { 
        
        sort(nums.begin(), nums.end());
        vector<int> onePath; 
        vector<vector<int>> ret; 
        vector<bool> visit(nums.size(), false); 
        backTrack(nums, visit, onePath, ret); 
        
        return ret;         
    }
    
    
    
    void backTrack(vector<int>& nums, vector<bool>& visit, vector<int>& onePath, vector<vector<int>>& ret)
    {
        if(onePath.size()==nums.size())
        {
            ret.push_back(onePath); 
            return;             
        }
        
        for(int i=0; i< nums.size(); i++)
        {
            if(visit[i])  continue; 
            if(i>0 && nums[i]==nums[i-1] && visit[i-1]==false) continue; 
            visit[i] = true; 
            onePath.push_back(nums[i]); 
            backTrack(nums, visit, onePath, ret); 
            onePath.pop_back(); 
            visit[i] = false; 
        }                
    }
        
};
```

### [N-Queens](https://leetcode.com/problems/n-queens/)

```cpp
// Some code
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        
        vector<int> oneResult; 
        vector<vector<string>> ret; 
        backTrac(n, 0, oneResult, ret); 
        return ret; 
    }
    
    
    
    void backTrac(int n, int level, vector<int>& oneResult, vector<vector<string>>& ret)
    {
        // start mean row. 
        if(level ==n)
        {
            vector<string> tmp(n);
           for(int i=0; i< n; i++)
           {
               for(int j =0; j< n; j++)
               {
                   if(oneResult[i]==j)
                       tmp[i].push_back('Q'); 
                   else
                       tmp[i].push_back('.');
               }
           }
           ret.push_back(tmp);  
            return; 
        }                        
        for(int i=0; i< n; i++)
        {
            if(canPutQueue(oneResult, i))
            {
                oneResult.push_back(i); 
                backTrac(n, level+1, oneResult, ret); 
                oneResult.pop_back(); 
            }
            
        }                
    }
    
    bool canPutQueue(vector<int>& oneResult, int pos)
    {
        int row = oneResult.size(); 
        // col i 
        // col can't be same. 
        for(int i=0; i< oneResult.size(); i++)
        {
            if(pos == oneResult[i])
                return false; 
        }
        //  diag
        for(int i=0; i< oneResult.size(); i++) // [i  oneResult[i]]
        {
            int icol = oneResult[i] +(row - i); 
            if(icol ==pos)
                return false; 
        }
        // 
        for(int i=oneResult.size()-1; i>=0 ; i--)
        {
            int icol = oneResult[i] - (row -i); 
            if(icol ==pos)
                return false; 
        }
        return true;         
    }
};
```

### N-Queens II

```cpp
// Some code
class Solution {
public:
    int totalNQueens(int n) {
        
    // Time O(n^n)  Space O(n)  Use 15 min.        
        vector<int> onePath;
        int ret=0; 
        
        backTrac(n, onePath, 0, ret); 
        return ret; 
        
    }
    
    
    void backTrac(int n, vector<int>& onePath, int row,  int& ret)
    {
        if(onePath.size()==n) // Find one possible solution. 
        {
            ret++; 
            return;             
        }            
        for(int col =0; col <n; col++) // loop every col and see if it is valid. 
        {
            if(isValid(onePath, row, col))
            {
                onePath.push_back(col); 
                backTrac(n, onePath, row+1, ret);
                onePath.pop_back(); 
            }
        }
    }
    
    bool isValid(vector<int>& onePath, int irow, int icol)
    {
        // vertical
        for(int i=0; i< onePath.size(); i++)
        {
            if(onePath[i] == icol)
                return false; 
            if(onePath[i]+(irow-i) == icol)
                return false; 
            if(onePath[i]-(irow-i) == icol)
                return false; 
        }        
        return true;         
    }
};

```

### [Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)

采用回溯方法，要注意一些减枝策略。另外递归函数的返回要注意。同时为了加速，当sum\_target\<sum，我们不需要从0开始搜索，因为之前的以前搜索过了。

```cpp
// Some code
// 
class Solution {
public:
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        
        //  get sum. 
        // check sum%k ==0 
        // first check if can find sum/k. 
        // 
        map<int,int> record;
        int sum =0; 
        for(int i=0; i< nums.size(); i++)
        {
             sum += nums[i];             
            record[nums[i]]++; 
        }
        if(sum%k!=0)
            return false; 
        sum = sum/k; 
        return backTrac(record, record.begin(), sum, 0, k,  0);         
    }
    
    bool backTrac(map<int,int>& nums, map<int,int>::iterator start, int sum_target, int count, int k, int sum)
    {
        // Find k subset. return true. 
        if(count ==k)
            return true; 
        
        for(auto it = start; it!= nums.end(); it++)
        {
            //  (*it).second means how many this number was left.  zero means all were used. 
            if((*it).second && (sum + (*it).first)<=sum_target)
            {
                sum+=(*it).first;
                (*it).second--;
                if(sum==sum_target && backTrac(nums, nums.begin(), sum_target, count+1, k, 0))
                   return true; 
                else
                {
                    if(backTrac(nums, it , sum_target,  count, k, sum))
                        return true; 
                }
                (*it).second++;
                sum -=(*it).first; 
            }                        
        }
        return false;         
    }
};  
```

## 练习

* [ ] [subsets](https://leetcode-cn.com/problems/subsets/)
* [ ] [subsets-ii](https://leetcode-cn.com/problems/subsets-ii/)
* [ ] [permutations](https://leetcode-cn.com/problems/permutations/)
* [ ] [permutations-ii](https://leetcode-cn.com/problems/permutations-ii/)

挑战题目

* [ ] [combination-sum](https://leetcode-cn.com/problems/combination-sum/)
* [ ] [letter-combinations-of-a-phone-number](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)
* [ ] [palindrome-partitioning](https://leetcode-cn.com/problems/palindrome-partitioning/)
* [ ] [restore-ip-addresses](https://leetcode-cn.com/problems/restore-ip-addresses/)
* [ ] [permutations](https://leetcode-cn.com/problems/permutations/)
