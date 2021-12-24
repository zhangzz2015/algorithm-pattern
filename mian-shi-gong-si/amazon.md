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

#### **aaa**&#x20;

****

####
