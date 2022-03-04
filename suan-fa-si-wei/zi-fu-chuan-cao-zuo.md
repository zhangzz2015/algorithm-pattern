# 字符串操作

### 字符串匹配方法

#### KMP方法

```cpp
// Some code+
class Solution {
public:
    int strStr(string haystack, string needle) {
        
        
        //  kmp method. 
        // calculate prefix. 
        
        if(haystack.size()< needle.size())
            return -1; 
        
        vector<int> prefix(needle.size(), 0); 
        int left =0; 
        int right =1; 
        
        // sliding windows. 
        while(right < needle.size())
        {
            while(left>0 && needle[left] != needle[right])
            {
                left = prefix[left-1]; 
            }
            
            if(needle[left]==needle[right])
                left ++;
            
            
            prefix[right] = left; 
            right++;             
        }
        
        
        left =0; 
        right =0; 
        
        while(left < haystack.size() && right < needle.size())
        {
            while(right>0 && haystack[left] != needle[right] )
            {
                right = prefix[right-1]; 
            } 
            
            if(haystack[left] == needle[right] )
                right++; 

            left++; 
        }
        
        if(right == needle.size())
        {
            return  left - right; 
        }
        else
            return -1; 
        
        
    }
};
```

#### Rolling hash方法

```cpp
// Some code
int hasDup(string& s, int val)
{
]et * 26 + s[i]; 
            ret = ret%prim; 
            baseVal *=26; 
            baseVal = baseVal%prim; 
        }
        
        record[ret].push_back(0); 
        for(int i = val ; i< s.size(); i++)
        {
            ret = (ret*26 + s[i] - baseVal*s[i-val]%prim + prim)%prim; 
            auto it = record.find(ret); 
            if(it!=record.end())
            {
                for(int j=0; j< (*it).second.size(); j++)
                {
                    if(isSameString(s, (*it).second[j], i-val+1, val))
                        return (*it).second[j]; 
                }
                record[ret].push_back(i-val+1); 
            }
            else
            {
                record[ret].push_back(i-val+1); 
            }
        }
        return -1; 
    }
    
    bool isSameString(string& s, int s1, int s2, int size)
    {
        for(int i=0; i< size; i++)
        {
            if(s[s1+i] != s[s2+i])
                return false;
        }
        return true; 
    }
```
