# Sampling方法

包括Reject sampling 和 **Fisher-Yates Shuffle 以及蓄水池算法**

蓄水池算法

计算复杂度高，空间复杂度低，核心逻辑，选择第i个数的概率 1/i \* （1 - 1/(i+1)） \* (1 - 1/(i+2)) \* (1 - 1/N) = 1/N，比较适合流数据处理，也可生成k/N 个随机数



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

### [Linked List Random Node](https://leetcode.com/problems/linked-list-random-node/)

```cpp
// Some code
class Solution {
public:
    
    ListNode* head_node; 
    Solution(ListNode* head) {
        head_node = head; 
    }
    
    int getRandom() {
        
        ListNode* current = head_node; 
        int result = current->val; 
        int num =0; 
        while(current)
        {
            num++; 
            int randval = rand()%num;
            if(randval ==0 ) // 1/num  to use this val.
            {
                result = current->val; 
            }            
            current = current->next; 
        }
        
        return result; 
        
    }
};
```
