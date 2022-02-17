# Sampling方法

包括Reject sampling 和 **Fisher-Yates Shuffle 以及蓄水池算法**

### **470.Implement Rand10() Using Rand7()**

使用7个数字表达出更多个数字，构造两位7进制，(rand7() - 1) \* 7 + rand7() - 1，可以取得0-48之间的数字。如得到0-39之间的数字，使用（num % 10 + 1）等概率映射出1-10. 如得到40-48之间的数字，重新选择...直到选择到0-39之间的数字

```cpp
// Some cc++
// transfer rand7 to rand10. 
class Solution {
public:
    int rand10() {        
//   [1  7]    (rand7()-1) * 7 + rand7() -1  [ 0   48]   discard >=40. 
//        obtain [0, 39] .  %10 to get 0 - 9.   +1 to get [1 10]
        int random = 49; 
        while(random>=40)
        {
            random = (rand7()-1)*7 + rand7() -1; 
        }
        
        return random%10+1; 
        
    }
};
```
