# 字符串操作

### 字符串匹配方法

#### KMP方法

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
