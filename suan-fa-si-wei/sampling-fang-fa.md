# Sampling方法

包括Reject sampling 和 **Fisher-Yates Shuffle 以及蓄水池算法**

### **470.Implement Rand10() Using Rand7()**

```cpp
// Some cc++
// transfer rand7 to rand10. 
class Solution {
public:
    int rand10() {
        
        //   [1  7]    (rand7()-1) * 7 + rand7() -1  [ 0   48]  >=40. descard.   
        int random = 49; 
        while(random>=40)
        {
            random = (rand7()-1)*7 + rand7() -1; 
        }
        
        return random%10+1; 
        
    }
};
```
