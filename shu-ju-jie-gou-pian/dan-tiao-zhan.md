# 单调栈

单调栈用来查找左边或者右边第一个大/小的元素

```cpp
// Some code
// calculate leftSmall and right small index. If it is -1, mean it hasnot. 
///  increasing stack. 
void findNextSmall(vector<int>& num)
{
   int n = nums.size(); 
   vector<int>  leftSmall(n, -1);  
   vector<int>  rightSmall(n, -1); 
   vector<int> stack; 
   for(int i=0; i< num.size(); i++) 
   {
      while(stack.size() && num[stack.back()]>num[i])
      {
          int topIndex = stack.back(); 
          rightSmall[topInex] = i; 
          stack.pop_back(); 
          if(stack.size())
            leftSmall[topIndex] = stack.back();           
      }
      stack.push_back(i); 
   }
   for(int i=stack.size()-1; i>0; i--)
   {
      leftSmall[i] = stack[i-1]; 
   }   
}

// Some code
// calculate left Large and right Large index. If it is -1, mean it hasnot. 
void findNextLarge(vector<int>& num)
{
   int n = nums.size();
   vector<int>  leftLarge(n, -1);  
   vector<int>  rightLarge(n, -1); 
   vector<int> stack; 
   for(int i=0; i< num.size(); i++) 
   {
      while(stack.size() && num[stack.back()]<num[i])
      {
          int topIndex = stack.back(); 
          rightLarge[topInex] = i; 
          stack.pop_back(); 
          if(stack.size())
            leftLarge[topIndex] = stack.back();           
      }
      stack.push_back(i); 
   }
   for(int i=stack.size()-1; i>0; i--)
   {
      leftLarge[i] = stack[i-1]; 
   }   
}

```
