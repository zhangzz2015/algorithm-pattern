# 二分搜索

## 二分搜索模板

给一个**有序数组**和目标值，找第一次/最后一次/任何一次出现的索引，如果没有出现返回-1

模板四点要素

* 1、初始化：left=0,  right=leng-1  尽量总是使用相同左闭右闭区间。
* 2、循环退出条件：left <= right &#x20;
* 3、比较中点和目标值：A\[mid] ==、 <、> target
* 4、判断最后两个元素是否符合：A\[start]、A\[end] ? target
* 5、要么在找到情况下返回，最后退出条件一定是right\<left，并且right和left一定是相邻，同时小于等于right是某一种情况，大于等于left是另外一种情况，找到的地方正好是发生转变的地方。
* 6、在分析退出时，我们可假设left = right，然后我们就能非常容易确定应该如何处理判断条件，同时返回究竟是left还是right。
* 当我们碰见有序或者部分有序，要考虑使用二分，或者在某一种地方发生分界。

时间复杂度 O(logn)，使用场景一般是有序数组的查找

典型示例

[binary-search](https://leetcode-cn.com/problems/binary-search/)

> 给定一个  n  个元素有序的（升序）整型数组  nums 和一个目标值  target  ，写一个函数搜索  nums  中的 target，如果目标值存在返回下标，否则返回 -1。

```cpp
// 二分搜索最常用模板 每个数都是unique的。
class Solution {
public:
    int search(vector<int>& nums, int target) {
        
        int left = 0; 
        int right = nums.size()-1; 
        while(left <=right )
        {
            int middle = left + (right -left)/2; 
            
            if(nums[middle] == target) 
                return middle; 
            else if(nums[middle] < target)
                left = middle+1; 
            else
                right = middle-1;                             
        }
        
        return -1; 
        
    }
};
```

### [search-for-range](https://www.lintcode.com/problem/search-for-a-range/description)

> 给定一个包含 n 个整数的排序数组，找出给定目标值 target 的起始和结束位置。 如果目标值不在数组中，则返回`[-1, -1]`

思路：核心点就是找第一个 target 的索引，和最后一个 target 的索引，所以用两次二分搜索分别找第一次和最后一次的位置

```go
func searchRange (A []int, target int) []int {
    if len(A) == 0 {
        return []int{-1, -1}
    }
    result := make([]int, 2)
    start := 0
    end := len(A) - 1
    for start+1 < end {
        mid := start + (end-start)/2
        if A[mid] > target {
            end = mid
        } else if A[mid] < target {
            start = mid
        } else {
            // 如果相等，应该继续向左找，就能找到第一个目标值的位置
            end = mid
        }
    }
    // 搜索左边的索引
    if A[start] == target {
        result[0] = start
    } else if A[end] == target {
        result[0] = end
    } else {
        result[0] = -1
        result[1] = -1
        return result
    }
    start = 0
    end = len(A) - 1
    for start+1 < end {
        mid := start + (end-start)/2
        if A[mid] > target {
            end = mid
        } else if A[mid] < target {
            start = mid
        } else {
            // 如果相等，应该继续向右找，就能找到最后一个目标值的位置
            start = mid
        }
    }
    // 搜索右边的索引
    if A[end] == target {
        result[1] = end
    } else if A[start] == target {
        result[1] = start
    } else {
        result[0] = -1
        result[1] = -1
        return result
    }
    return result
}
```

### [search-insert-position](https://leetcode-cn.com/problems/search-insert-position/)

> 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

```go
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        
        int left=0; 
        int right = nums.size()-1; 
        
        while(left <= right){
            int middle = left  + (right -left)/2; 
            
            if(target == nums[middle])
                return middle; 
            else if(target < nums[middle])
                right = middle-1; 
            else
                left = middle+1;                                             
        }
        
        // [right left] 
        return left;         
    }
    
};
```

### [search-a-2d-matrix](https://leetcode.com/problems/search-a-2d-matrix/submissions/)

> 编写一个高效的算法来判断  m x n  矩阵中，是否存在一个目标值。该矩阵具有如下特性：
>
> * 每行中的整数从左到右按升序排列。
> * 每行的第一个整数大于前一行的最后一个整数。

```go
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        
        
    int row = matrix.size(); 
    int col = matrix[0].size(); 
    int left =0;     
    int right = row*col -1;         
    while(left <=right){
        int middle = left + (right - left)/2; 
        //  irow * col + icol
        int irow = middle/col; 
        int icol = middle%col; 
        if(matrix[irow][icol] == target)
            return true; 
        else if(matrix[irow][icol] < target)
            left = middle+1; 
        else
            right = middle-1;             
    }        
    return false;                     
    }
};
```

### [first-bad-version](https://leetcode-cn.com/problems/first-bad-version/)

> 假设你有 n 个版本 \[1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。 你可以通过调用  bool isBadVersion(version)  接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

```go
class Solution {
public:
    int firstBadVersion(int n) {
        int left =1; 
        int right =n; 
        while(left<=right)
        {
            int middle = left + (right - left)/2; 
            
            if(isBadVersion(middle)){
                right =  middle - 1; 
            }
            else{
                left = middle + 1; 
            }            
        }
        //  [right left]
        
        return left;                 
    }
};
```

### [find-minimum-in-rotated-sorted-array](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

> 假设按照升序排序的数组在预先未知的某个点上进行了旋转( 例如，数组  \[0,1,2,4,5,6,7] 可能变为  \[4,5,6,7,0,1,2] )。 请找出其中最小的元素。

```go
func findMin(nums []int) int {
    // 思路：/ / 最后一个值作为target，然后往左移动，最后比较start、end的值
    if len(nums) == 0 {
        return -1
    }
    start := 0
    end := len(nums) - 1

    for start+1 < end {
        mid := start + (end-start)/2
        // 最后一个元素值为target
        if nums[mid] <= nums[end] {
            end = mid
        } else {
            start = mid
        }
    }
    if nums[start] > nums[end] {
        return nums[end]
    }
    return nums[start]
}
```

### [find-minimum-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

> 假设按照升序排序的数组在预先未知的某个点上进行了旋转 ( 例如，数组  \[0,1,2,4,5,6,7] 可能变为  \[4,5,6,7,0,1,2] )。 请找出其中最小的元素。(包含重复元素)

```go
func findMin(nums []int) int {
    // 思路：跳过重复元素，mid值和end值比较，分为两种情况进行处理
    if len(nums) == 0 {
        return -1
    }
    start := 0
    end := len(nums) - 1
    for start+1 < end {
        // 去除重复元素
        for start < end && nums[end] == nums[end-1] {
            end--
        }
        for start < end && nums[start] == nums[start+1] {
            start++
        }
        mid := start + (end-start)/2
        // 中间元素和最后一个元素比较（判断中间点落在左边上升区，还是右边上升区）
        if nums[mid] <= nums[end] {
            end = mid
        } else {
            start = mid
        }
    }
    if nums[start] > nums[end] {
        return nums[end]
    }
    return nums[start]
}
```

### [search-in-rotated-sorted-array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

> 假设按照升序排序的数组在预先未知的某个点上进行了旋转。 ( 例如，数组  \[0,1,2,4,5,6,7]  可能变为  \[4,5,6,7,0,1,2] )。 搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回  -1 。 你可以假设数组中不存在重复的元素。

```go
func search(nums []int, target int) int {
    // 思路：/ / 两条上升直线，四种情况判断
    if len(nums) == 0 {
        return -1
    }
    start := 0
    end := len(nums) - 1
    for start+1 < end {
        mid := start + (end-start)/2
        // 相等直接返回
        if nums[mid] == target {
            return mid
        }
        // 判断在那个区间，可能分为四种情况
        if nums[start] < nums[mid] {
            if nums[start] <= target && target <= nums[mid] {
                end = mid
            } else {
                start = mid
            }
        } else if nums[end] > nums[mid] {
            if nums[end] >= target && nums[mid] <= target {
                start = mid
            } else {
                end = mid
            }
        }
    }
    if nums[start] == target {
        return start
    } else if nums[end] == target {
        return end
    }
    return -1
}
```

注意点

> 面试时，可以直接画图进行辅助说明，空讲很容易让大家都比较蒙圈

### [search-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

> 假设按照升序排序的数组在预先未知的某个点上进行了旋转。 ( 例如，数组  \[0,0,1,2,2,5,6]  可能变为  \[2,5,6,0,0,1,2] )。 编写一个函数来判断给定的目标值是否存在于数组中。若存在返回  true，否则返回  false。(包含重复元素)

```go
func search(nums []int, target int) bool {
    // 思路：/ / 两条上升直线，四种情况判断，并且处理重复数字
    if len(nums) == 0 {
        return false
    }
    start := 0
    end := len(nums) - 1
    for start+1 < end {
        // 处理重复数字
        for start < end && nums[start] == nums[start+1] {
            start++
        }
        for start < end && nums[end] == nums[end-1] {
            end--
        }
        mid := start + (end-start)/2
        // 相等直接返回
        if nums[mid] == target {
            return true
        }
        // 判断在那个区间，可能分为四种情况
        if nums[start] < nums[mid] {
            if nums[start] <= target && target <= nums[mid] {
                end = mid
            } else {
                start = mid
            }
        } else if nums[end] > nums[mid] {
            if nums[end] >= target && nums[mid] <= target {
                start = mid
            } else {
                end = mid
            }
        }
    }
    if nums[start] == target || nums[end] == target {
        return true
    }
    return false
}
```

### [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```cpp
// Some code
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        
     vector<int> ret;    
     ret.push_back(binaryLowerBound(nums, target));
     ret.push_back(binaryUpperBound(nums, target));
        
        return ret; 
      
        
    }
    // return [right left]
    // boundary point right<target left>=target
    int binaryLowerBound(vector<int>& nums, int target)
    {
        int left =0; 
        int right = nums.size()-1; 
        
        while(left <=right)
        {
            int middle = left + (right - left)/2; 
            
            if(nums[middle]<target)
                left = middle+1; 
            else
                right = middle-1;                             
        }        
        // [right left]  return.  
        // then we return left. 
        if(left==nums.size()|| nums[left]!=target)
            return -1; 
        else
            return left;             
    }
    // return [right left]
    // boundary point right<=target left>target
    int binaryUpperBound(vector<int>& nums, int target)
    {
        int left =0; 
        int right = nums.size()-1; 
        
        while(left <=right)
        {
            int middle = left + (right - left)/2;             
            if(nums[middle]<=target)
                left = middle+1; 
            else
                right = middle-1;                             
        }        
        // [right left]  return.  right. 
        if(right==-1|| nums[right]!=target)
            return -1; 
        else
            return right;             
    }    
};
```

## 总结

二分搜索核心四点要素（必背&理解）

* 1、初始化：start=0、end=len-1
* 2、循环退出条件：start + 1 < end
* 3、比较中点和目标值：A\[mid] ==、 <、> target
* 4、判断最后两个元素是否符合：A\[start]、A\[end] ? target

## 练习题

* [ ] [search-for-range](https://www.lintcode.com/problem/search-for-a-range/description)
* [ ] [search-insert-position](https://leetcode-cn.com/problems/search-insert-position/)
* [ ] [search-a-2d-matrix](https://leetcode-cn.com/problems/search-a-2d-matrix/)
* [ ] [first-bad-version](https://leetcode-cn.com/problems/first-bad-version/)
* [ ] [find-minimum-in-rotated-sorted-array](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)
* [ ] [find-minimum-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)
* [ ] [search-in-rotated-sorted-array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)
* [ ] [search-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)
