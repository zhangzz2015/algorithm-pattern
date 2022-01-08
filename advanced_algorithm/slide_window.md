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

[minimum-window-substring](https://leetcode.com/problems/minimum-window-substring/)

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

[permutation-in-string](https://leetcode-cn.com/problems/permutation-in-string/)

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

[find-all-anagrams-in-a-string](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

> 给定一个字符串  \*\*s \*\*和一个非空字符串  **p**，找到  \*\*s \*\*中所有是  \*\*p \*\*的字母异位词的子串，返回这些子串的起始索引。

```go
func findAnagrams(s string, p string) []int {
    win := make(map[byte]int)
	need := make(map[byte]int)
	for i := 0; i < len(p); i++ {
		need[p[i]]++
	}
	left := 0
	right := 0
	match := 0
    ans:=make([]int,0)
	for right < len(s) {
		c := s[right]
		right++
		if need[c] != 0 {
			win[c]++
			if win[c] == need[c] {
				match++
			}
		}
		// 当窗口长度大于字符串长度，缩紧窗口
		for right-left >= len(p) {
			// 当窗口长度和字符串匹配，并且里面每个字符数量也匹配时，满足条件
			if right-left == len(p)&& match == len(need) {
				ans=append(ans,left)
			}
			d := s[left]
			left++
			if need[d] != 0 {
				if win[d] == need[d] {
					match--
				}
				win[d]--
			}
		}
	}
	return ans
}
```

[longest-substring-without-repeating-characters](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

> 给定一个字符串，请你找出其中不含有重复字符的   最长子串   的长度。 示例  1:
>
> 输入: "abcabcbb" 输出: 3 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

```go
func lengthOfLongestSubstring(s string) int {
    // 滑动窗口核心点：1、右指针右移 2、根据题意收缩窗口 3、左指针右移更新窗口 4、根据题意计算结果
    if len(s)==0{
        return 0
    }
    win:=make(map[byte]int)
    left:=0
    right:=0
    ans:=1
    for right<len(s){
        c:=s[right]
        right++
        win[c]++
        // 缩小窗口
        for win[c]>1{
            d:=s[left]
            left++
            win[d]--
        }
        // 计算结果
        ans=max(right-left,ans)
    }
    return ans
}
func max(a,b int)int{
    if a>b{
        return a
    }
    return b
}
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
