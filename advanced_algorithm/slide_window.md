# 滑动窗口思想

## 模板

```cpp
/* 滑动窗口算法框架 */
void slidingWindow(string s, string t) {
    unordered_map<char, int> need, window;
    for (char c : t) need[c]++;

    int left = 0, right = 0;
    int valid = 0;
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 进行窗口内数据的一系列更新
        ...

        /*** debug 输出的位置 ***/
        printf("window: [%d, %d)\n", left, right);
        /********************/

        // 判断左侧窗口是否要收缩
        while (window needs shrink) {
            // d 是将移出窗口的字符
            char d = s[left];
            // 左移窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
        
        // 右移窗口
        right++;        
    }
}
```

需要变化的地方

* 1、右指针右移之后窗口数据更新
* 2、判断窗口是否要收缩
* 3、左指针右移之后窗口数据更新
* 4、根据题意计算结果

## 示例

### [minimum-window-substring](https://leetcode.com/problems/minimum-window-substring/)

> 给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字母的最小子串

思路：使用标准sliding window，利用hash map，记录是否包含所有S1的字符，先移动right，找到一个可行解，再移动left，看该可行解可以保持多久，同时找到最小的可行解。

```go
class Solution {
public:
    string minWindow(string s, string t) {
        
        if(t.size()> s.size())
            return ""; 
        vector<int> t_map(256, 0); 
        vector<int> s_map(256, 0); 
        for(int i=0; i< t.size(); i++)
            t_map[t[i]]++; 
        int left =0; 
        int right = 0; 
        int count =0;
        int ret = INT_MAX; 
        int ret_start = -1; 
        while(right < s.size()){
            
            s_map[s[right]]++;
            if(s_map[s[right]] <= t_map[s[right]])
                count++; 
            
            while(count==t.size())
            {
               if(right - left + 1 < ret)
               {
                  ret = right - left+1; 
                  ret_start = left;  
               }
               s_map[s[left]]--; 
               if(s_map[s[left]]< t_map[s[left]])
                   count--; 
                left++; 
            }
            
            right++; 
        }
        
        return ret == INT_MAX? "" : s.substr(ret_start, ret);                         
    }
};
```

### [permutation-in-string](https://leetcode-cn.com/problems/permutation-in-string/)

> 给定两个字符串  **s1**  和  **s2**，写一个函数来判断  **s2**  是否包含  \*\*s1 \*\*的排列。
>
> 思路：使用和S1固定大小的窗口，在该窗口大小内，生成hash map是否相同，来判断是否是一个排列。利用count来判断两个hash map是否相同。

```go
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        
        if(s1.size()>s2.size())
            return false; 
        
        vector<int> s1_map(26, 0); 
        vector<int> s2_map(26, 0); 
        
        for(int i=0; i< s1.size(); i++)
            s1_map[s1[i]-'a']++; 
        
        int count =0; 
        for(int i=0; i< s1.size(); i++)
        {
            int index = s2[i] - 'a'; 
            s2_map[index]++; 
            if(s2_map[index]<=s1_map[index])
                count++; 
        }
        if(count == s1.size())
            return true; 
        
        for(int i= s1.size(); i< s2.size(); i++)
        {
            int index = s2[i] - 'a'; 
            s2_map[index]++; 
            if(s2_map[index]<=s1_map[index])
                count++; 
            index = s2[i-s1.size()] - 'a'; 
            s2_map[index]--; 
            if(s2_map[index]<s1_map[index])
                count--; 
            
            if(count == s1.size())
                return true;             
        }
        
        return false; 
        
    }
};
```

### [438 Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

> 给定一个字符串  \*\*s \*\*和一个非空字符串  **p**，找到  \*\*s \*\*中所有是  \*\*p \*\*的字母异位词的子串，返回这些子串的起始索引。

```go
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        
        // use fix size sliding windows. 
        
        vector<int> s_map(26,0); 
        vector<int> p_map(26,0); 
        if(s.size()<p.size())
            return {};
        
        vector<int> ret; 
        
        int count=0; 
        for(int i=0; i< p.size(); i++)
           p_map[p[i]-'a']++; 
        
        for(int i=0; i< p.size(); i++)
        {
            int index = s[i]-'a';
            s_map[index]++; 
            if(s_map[index] <=p_map[index])
                count++;             
        }
        if(count == p.size())
            ret.push_back(0); 
        
        for(int i=p.size(); i<s.size(); i++)
        {
            int index = s[i]-'a'; 
            s_map[index]++; 
            if(s_map[index] <=p_map[index])
                count++;             
            index = s[i-p.size()]-'a'; 
            s_map[index]--; 
            if(s_map[index]< p_map[index])
                count--; 
            if(count==p.size())
                ret.push_back(i-p.size()+1); 
        }
        
        return ret;                 
    }
};
```

### [longest-substring-without-repeating-characters](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

> 给定一个字符串，请你找出其中不含有重复字符的   最长子串   的长度。 示例  1:
>
> 输入: "abcabcbb" 输出: 3 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

思路：利用hashmap来维护是否右重复字符。当有重复字符时候，缩小窗口，直到没有重复为止，则表明\[left, right]为一个没有重复字符的字符串。

```go
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        
        vector<int> record(256, 0); 
        
        int left =0; 
        int right =0; 
        int ret =0; 
        while(right < s.size())
        {
            record[s[right]]++; 
            while(record[s[right]]>1 )
            {
                record[s[left]]--; 
                left++; 
            }
            
            ret = max(ret, (right - left+1)); 
            
            right++; 
        }
        
        return ret; 
        
    }
};
```

### Subarrays with K Different Integers

```cpp
// Some codec
class Solution {
public:
    int subarraysWithKDistinct(vector<int>& nums, int k) {
        
        // Time O(n)  Space O(n)   Use  30 min. 
        // transfer this question.   k=<subarray_val      k+1=<subarry_val
        return maxsubarry(nums, k) -  maxsubarry(nums, k+1);
        
    }
    
    int maxsubarry(vector<int>& nums, int k)
    {
        
        unordered_map<int,int> record; 
        int left =0; 
        int right =0; 
        int ret =0; 
        while(right < nums.size())
        {
            record[nums[right]]++;             
            // shrink left. from left to right. min 
            while(record.size()==k )
            {
                // from right to end. all are substring meet requrient. 
                ret +=(nums.size() - right); 
                record[nums[left]]--; 
                if(record[nums[left]]==0)
                    record.erase(nums[left]); 
                left++; 
            }                                
            right++; 
            
        }
        
        return ret; 
    }
};
```

## 总结

* 和双指针题目类似，更像双指针的升级版，滑动窗口核心点是维护一个窗口集，根据窗口集来进行处理
* 核心步骤
  * right 右移
  * 收缩
  * left 右移
  * 求结果

## 练习

* [ ] [minimum-window-substring](https://leetcode-cn.com/problems/minimum-window-substring/)
* [ ] [permutation-in-string](https://leetcode-cn.com/problems/permutation-in-string/)
* [ ] [find-all-anagrams-in-a-string](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)
* [ ] [longest-substring-without-repeating-characters](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
