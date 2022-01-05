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

####
