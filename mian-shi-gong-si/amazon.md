# Amazon

### OA questions.

#### **Amazon OA password strength**

Given a string password, find the strength of that password. The strength of a password, consisting only lowercase english letters only, is calculated as the sum of the number of distinct characters present in all possible substrings of that password. Example:- password = "good" possible sub string and count of distinct characters are g = 1 o = 1 o = 1 d = 1 go = 2 oo = 1 od = 2 goo = 2 ood = 2 good = 3 1+1+1+1+2+1+2+2+2+3 = 16

```cpp
// Some code
// A   1
// A B   prev + (i -(-1)) = 3; 
// A B A 
int passwdStrengh(string s)
{ 
     vector<int> pos(26,-1); 
     int cur =0; 
     int res =0; 
     for(int i=0; i< s.size(); i++)
     {
        int index = s[i]-'a';
        cur += (i - pos[index]);
        res += cur; 
     }
     return res; 
}
```

#### maximum teams amazon ****&#x20;

Q1 was about given an Array of numbers, for example {5, 1, 2, 1, 4, 5} which represent skills of employees..(Skills\[i]) How many teams you can form, given the Teamsize = 3 and MaxDiff = 2 which is the maximum difference allowed in skills&#x20;

Example 1 = With {5, 1, 2, 1, 4, 5} and given Teamsize = 3 and MaxDiff = 2. You can form _**2 teams**_ \[2, 1, 1] and \[4, 5, 5].\
Example 2 : 0 teams can be formed with \[9, 3, 5, 7, 1] and int teamSize = 2; int maxDiff = 1

```
// Some code
// first sort it. 
int maxTeam(vector<int>& skill, int max_diff, int teamsize)
{
     int left =0; 
     int right = teamsize-1; 
     int ret =0; 
     while(right<skill.size())
     {
          if(skill[right] - skill[left] <=max_diff)
          {
             ret++; 
             left+=teamSize; 
             right+=teamSize; 
          }
          else
          {
           left++;
           right++; 
          }          
     }
     return ret; 
}
```

**Valid string**

There are 3 rules for a valid string:

1. An empty string is valid
2. You can add same character to a valid string X, and create another valid string yXy
3. You can concatenate two valid strings X and Y, so XY will also be valid.
4. Ex: vv, xbbx, bbccdd, xyffyxdd are all valid.

```
// Some code use stack. 
bool isValid(string s) 
{
   vector<char> stack; 
   for(int i=0; i < s.size(); i++) {
     
     if(stack.size() && s[i] == stack.back())
     {
        stack.pop_back(); 
     }
     else
        stack.push_back(s[i]);       
   }
   
   return stack.empty(); 
}

```

![](../.gitbook/assets/b412b20e-0223-46fc-888b-891763017afe\_1640099193.5917742.png)

```cpp
// Some code
int countDecreasingRatings(vector<int>& ratings) {
// Write your code here

  int n = ratings.size();    
   if(n ==0) return 0;    
   int left=0; 
   int right =1;
   int result =1; 
    while(right < ratings.size())
    {
       if(ratings[right-1]-ratings[right]==1)
       {
          result +=(right-left+1) 
       }
       else
       {
          result +=1; 
          left = right; 
       }
       right++; 
    }
    return result;
}
```

![](../.gitbook/assets/9ba1a1dc-9e9f-44e9-8842-c28274c9cf83\_1640099294.8215818.png)

[2104. Sum of Subarray Ranges](https://leetcode.com/problems/sum-of-subarray-ranges/)

```cpp
// Some code
// min    1 2 3   1 * 3 + 2*2 + 3*1 
// max    1 2 3   1*1 + 2*2 + 3* 3 
// method 1. Use dp. 
class Solution {
public:
    long long subArrayRanges(vector<int>& nums) {
        
        // [i j]  min max
        // use dp. 
        vector<vector<int>> dpmin(nums.size(), vector<int>(nums.size())); 
        vector<vector<int>> dpmax(nums.size(), vector<int>(nums.size())); 
        //  dpmin[i][j] = min(f(i), dp[i+1][j]) ; 
        //  dpmax[i][j] = max(f(i), dpMax[i+1][j]);         
        long long ret =0; 
        for(int i=0; i< nums.size(); i++)
        {
            for(int j=i; j < nums.size(); j++)
            {
                if(i==0)
                {
                    dpmin[j-i][j] = nums[j]; 
                    dpmax[j-i][j] = nums[j]; 
                }
                else
                {
                    dpmin[j-i][j] = min(nums[j], dpmin[j-i][j-1]); 
                    dpmax[j-i][j] = max(nums[j], dpmax[j-i][j-1]); 
                    ret += (dpmax[j-i][j] - dpmin[j-i][j]); 
                }
            }
        }        
        return ret;                 
    }
};    
```
