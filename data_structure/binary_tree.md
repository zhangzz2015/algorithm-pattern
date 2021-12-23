# 二叉树

## 知识点

### 二叉树遍历

**前序遍历**：**先访问根节点**，再前序遍历左子树，再前序遍历右子树 **中序遍历**：先中序遍历左子树，**再访问根节点**，再中序遍历右子树 **后序遍历**：先后序遍历左子树，再后序遍历右子树，**再访问根节点**

注意点

* 以根访问顺序决定是什么遍历
* 左子树都是优先右子树

#### 前序递归

先访问根节点，再左节点，右节点。

```cpp
void preorderTraversal(TreeNode * root, vector<int>& ret)  
{
    if(root==NULL)
        return; 

    // 先访问根再访问左右
    // cout<<"val:" << root->val << "\n";
    ret.push_back(root->val);
    preorderTraversal(root->left);
    preorderTraversal(root->right); 
}
```

#### 中序递归

先访问左节点，再根节点，右节点。

```cpp
void inorderTranversal(TreeNode* root, vector<int>& ret)
{
     if(root==NULL)
        return ; 
     
    // visit left, then root, last visit right.  
    inorderTranversal(root->left, ret); 
    ret.push_back(root->val); 
    inorderTranversal(root->right, ret);      
}

```

#### 后序递归

先访问左节点，再右节点，最后根节点。

```cpp
void postTranversal(TreeNode* root, vector<int>& ret)
{
     if(root==NULL)
        return ; 
     
    // visit left, then root, last visit right.  
    postTranversal(root->left, ret); 
    postTranversal(root->right, ret);
    ret.push_back(root->val);           
}
```

#### 前序非递归

使用双色方法，该方法可统一前序，中序，和后序，可递归相同。

```cpp
// V3：通过非递归遍历
void preorderTraversal(TreeNode* root, vector<int>& ret) {
    // 非递归
    if (root==NULL)
        return;
        
    vector<pair<TreeNode*, bool>> stack; 
    
    stack.push_back(make_pair(root, false) ); 

    while(stack.size())
    {
        pair<TreeNode*, bool> topNode= stack.back(); 
        stack.pop_back(); 
        if(topNode.first == NULL)  continue; 
        
        if(topNode.second == false)
        {
            stack.push_back(make_pair(topNode, true)); 
            stack.push_back(make_pair(topNode->left, false)); 
            stack.push_back(make_pair(topNode->right, false)); 
        }
        else
        {
           ret.push_bac(topNode->val); 
        }        
    }
}
```

#### 中序非递归

```cpp
void inorderTraversal(TreeNode* root, vector<int>& ret) {
    // 非递归
    if (root==NULL)
        return;
        
    vector<pair<TreeNode*, bool>> stack; 
    
    stack.push_back(make_pair(root, false) ); 

    while(stack.size())
    {
        pair<TreeNode*, bool> topNode= stack.back(); 
        stack.pop_back(); 
        if(topNode.first == NULL)  continue; 
        
        if(topNode.second == false)
        {
            stack.push_back(make_pair(topNode->left, false));         
            stack.push_back(make_pair(topNode, true)); 
            stack.push_back(make_pair(topNode->right, false)); 
        }
        else
        {
           ret.push_bac(topNode->val); 
        }        
    }
}
```

#### 后序非递归

```cpp
void inorderTraversal(TreeNode* root, vector<int>& ret) {
    // 非递归
    if (root==NULL)
        return;
        
    vector<pair<TreeNode*, bool>> stack; 
    
    stack.push_back(make_pair(root, false) ); 

    while(stack.size())
    {
        pair<TreeNode*, bool> topNode= stack.back(); 
        stack.pop_back(); 
        if(topNode.first == NULL)  continue; 
        
        if(topNode.second == false)
        {
            stack.push_back(make_pair(topNode->left, false));          
            stack.push_back(make_pair(topNode->right, false)); 
            stack.push_back(make_pair(topNode, true));            
        }
        else
        {
           ret.push_bac(topNode->val); 
        }        
    }
}
```

注意点

* 核心就是：根节点必须在右节点弹出之后，再弹出

#### DFS 深度搜索-从上到下-类似前序遍历

```cpp
struct Node { 
	int val; 
	struct Node* left; 
	struct Node* right; 
}; 

void preorderTraversal(TreeNode* root) 
{
    vector<int> result; 
    dfs(root, result);
    return result;
}

// V1：深度遍历，结果指针作为参数传入到函数内部
void dfs(TreeNode * root, vector<int>& result) {
    if (root == NULL)
        return; 
    result.push_back(root->val)); 
    dfs(root.left, result);
    dfs(root.right, result);
}
```

#### DFS 深度搜索-从下向上（分治法）-类似后序遍历

```cpp
// V2：通过分治法遍历
vector<int> preorderTraversal(root *TreeNode) {
    vector<int> result = divideAndConquer(root); 
    return result;
}
vector<int> divideAndConquer(root *TreeNode) {
    // 返回条件(null & leaf)
    if root == nil {
        return {};
    }
    // 分治(Divide)
    vector<int> left  = divideAndConquer(root->left);
    vector<int> right = divideAndConquer(root->right);
    // 合并结果(Conquer)
    vector<int> result; 
    result.push_back(root->val); 
    result.insert(result.end(), left.begin(), left.end());
    result.insert(result.end(), right.begin(), right.end());    
    return result;
}
```

注意点：

> DFS 深度搜索（从上到下） 和分治法区别：前者一般将最终结果通过指针参数传入，后者一般递归返回结果最后合并

#### BFS 层次遍历

利用队列，维护每一层的遍历顺序。

```cpp
void levelOrder(root *TreeNode, vector<vector<int>>& result) {
    // 通过上一层的长度确定下一层的元素
    if (root == NULL) {
        return; 
    }
    queue<TreeNode*> que; 
    que.push(root);
    int level =0; 
    while(que.size())
    {
       int iSize = que.size(); 
       que.resize(level+1); 
       
       for(int i=0; i<  iSize; i++)
       {
          TreeNode* topNode = que.front(); 
          que.pop(); 
          que[level].push_back(topNode->val); 
          
          if(topNode->left)
             que.push(topNode->left));
          if(topNode->right)
             que.push(topNode->right));    
       } 
       level++;              
    }
    return ;
}
```

### 分治法应用

先分别处理局部，再合并结果

适用场景

* 快速排序
* 归并排序
* 二叉树相关问题

分治法模板

* 递归返回条件
* 分段处理
* 合并结果

```go
 traversal(root *TreeNode) ResultType  {
    // nil or leaf
    if root == nil {
        // do something and return
    }

    // Divide
    ResultType left = traversal(root.Left)
    ResultType right = traversal(root.Right)

    // Conquer
    ResultType result = Merge from left and right

    return result
}
```

#### 典型示例

```cpp
// V2：通过分治法遍历二叉树
vector<int> postorderTraversal(root *TreeNode)  {
    result := divideAndConquer(root)
    return result
}
vector<int> divideAndConquer(root *TreeNode) {
    if (root == NULL) {
        return {};
    }
    // 分治(Divide)
    vector<int> leftVec = divideAndConquer(root.Left)
    vector<int> rightVec = divideAndConquer(root.Right)
    vector<int> result; 
    result.push_back(root.Val);
    result.insert(result.end(), leftVec.begin(), leftVec.end());
    result.insert(result.end(), rightVec.begin(), rightVec.end());
    return result;
}
```

#### 归并排序: 使用Bottom up方法&#x20;

```cpp
void MergeSort(vector<int>& nums) 
{
    return mergeSort(nums, 0, nums.size()-1); 
}
void mergeSort(vector<int>& nums, int left, int right)  
{
    if(left>=right)
       return ; 
    int middle = left + (right - left)/2 ; 
    mergeSort(nums, left, middle);
    mergeSort(nums, middle+1, right);

    merge(nums, left, middle, right); 
    return result
}
// use two pointer in place. 
void merge(vecotr<int>& nums, int left, int middle, int right) 
{
   int left1  = left; 
   int right1 = middle; 
   int left2 = middle+1; 
   int right2 = right; 
   
   while(left1<=right1 && left2<=right2)
   {
       if(nums[left1] <=nums[left2])
       {
           left++; 
       }
       else
       {
           // switch. 
           int tmp = num[left2]; 
           // move right one step. 
           for(int i= left2; i>left1; i--)
           {
              num[i] = num[i-1];  
           }
           num[left1] = tmp; 
           left1++;
           right1++;  
           left2++; 
       } 
    }    
}
```

注意点

> 递归需要返回结果用于合并

#### 快速排序 ：

* 使用top down 方法
* 该方法需要使用随机选择交换主元。否则最差情况O(n^2)，平均为O(nlogn)
* 该方法也可用来找到最大的第n个元素

```cpp
void QuickSort(vector<int>& nums){
	// 思路：把一个数组分为左右两段，左段小于右段，类似分治法没有合并过程
	quickSort(nums, 0, nums,size()); 
	return nums

}
// 原地交换，所以传入交换索引
void quickSort(vector<int>& nums, int left, int right) 
{
	if(left>=right)
	    return; 
	// choose random as pivot.  If not. worst case is O(n^2)
	int pivot = rand()%(right-left+1)+left; 
	//  
	swap(nums[pivot], nums[left]); 
	int middle = partition(nums, start, end)
	quickSort(nums, left, middle-1)
	quickSort(nums, middle+1, right);
}
// partition.  use pivot and split left side and right. 
//  pivot is put into middle place. left side < pivot. 
//                            right side >= pivot. 
int partition(vector<int>& ret, int left, int right) 
{
   int  pivot = ret[left]; 
   while(left<right)
   {
       while(left<right && ret[right]>pivot)
       {
           right--; 
       }
       swap(ret[left],  ret[right]); 
       while(left<right && ret[left]<=pivot)
       {
           left++; 
       }
       swap(ret[left], ret[right]);        
   }
   
   ret[left] = pivot; 
   return left; 
}

```

注意点：

> 快排由于是原地交换所以没有合并过程 传入的索引是存在的索引（如：0、length-1 等），越界可能导致崩溃

常见题目示例

#### maximum-depth-of-binary-tree

[maximum-depth-of-binary-tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

> 给定一个二叉树，找出其最大深度。

思路：分治法，后序遍历，bottom up，先算处左边最大深度，再计算右边的最大深度，比较两边，返回较大一边加上1个root，返回。终止条件，root为空的情况，返回0。

```cpp
int maxDepth(root *TreeNode)
{
   /// 终止条件
    if (root == NULL) {
        return 0
    }
     // 计算左边
    int ileft  = maxDepth(root.Left); 
    // 计算右边
    int iright = maxDepth(root.Right); 

    // conquer：合并左右子树结果
    return max(left, right) + 1;
}
```

#### balanced-binary-tree

[balanced-binary-tree](https://leetcode-cn.com/problems/balanced-binary-tree/)

> 给定一个二叉树，判断它是否是高度平衡的二叉树。

思路：分治法，后序遍历，左边平衡 && 右边平衡 && 左右两边高度 <= 1， 因为需要返回是否平衡及高度。如果某种情况是不平衡，可直接返回false。考虑三种情况，左边不平衡，右边不平衡，或者左右高度差大于1，都直接返回false，当返回true时，同时计算root的高度。本题目需要从孩子节点传入两个信息，1个为孩子是否平衡，同时还需要传入孩子节点高度。当分析递归问题时，需要考虑问题如下，需要从parent传入什么信息，需要从孩子收集什么信息。当需要parent信息，需要前序，需要孩子信息，则为后序。

```cpp
bool isBalanced(root *TreeNode) {
    if maxDepth(root) == -1 {
        return false
    }
    return true
}

bool maxDepth(root *TreeNode, int& depth) int {
    // check
    if (root == NULL) {
        depth = 0; 
        return true; 
    }
    int leftDepth; 
    int rightDepth; 
    bool bleft =  maxDepth(root->Left, leftDepth);
    if(bleft==false)
      return false; 
    bool bright= maxDepth(root->Right, rightDepth);
    if(bright == false)
      return false; 
    if(abs(leftDepth - rightDepth)>1) 
       return false; 
    depth = max(leftDepth, rightDepth) +1; 
    return true;         
}
```

注意

> 一般工程中，结果通过两个变量来返回，不建议用一个变量表示两种含义

#### binary-tree-maximum-path-sum

[binary-tree-maximum-path-sum](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

> 给定一个**非空**二叉树，返回其最大路径和。

思路：分治法，分为三种情况：左子树最大路径和最大，右子树最大路径和最大，左右子树最大加根节点最大，需要保存两个变量：一个保存子树最大路径和，一个保存左右加根节点和，然后比较这个两个变量选择最大值即可

```go
type ResultType struct {
    SinglePath int // 保存单边最大值
    MaxPath int // 保存最大值（单边或者两个单边+根的值）
}
func maxPathSum(root *TreeNode) int {
    result := helper(root)
    return result.MaxPath
}
func helper(root *TreeNode) ResultType {
    // check
    if root == nil {
        return ResultType{
            SinglePath: 0,
            MaxPath: -(1 << 31),
        }
    }
    // Divide
    left := helper(root.Left)
    right := helper(root.Right)

    // Conquer
    result := ResultType{}
    // 求单边最大值
    if left.SinglePath > right.SinglePath {
        result.SinglePath = max(left.SinglePath + root.Val, 0)
    } else {
        result.SinglePath = max(right.SinglePath + root.Val, 0)
    }
    // 求两边加根最大值
    maxPath := max(right.MaxPath, left.MaxPath)
    result.MaxPath = max(maxPath,left.SinglePath+right.SinglePath+root.Val)
    return result
}
func max(a,b int) int {
    if a > b {
        return a
    }
    return b
}
```

#### lowest-common-ancestor-of-a-binary-tree

[lowest-common-ancestor-of-a-binary-tree](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

> 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

思路：分治法，有左子树的公共祖先或者有右子树的公共祖先，就返回子树的祖先，否则返回根节点

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    // check
    if root == nil {
        return root
    }
    // 相等 直接返回root节点即可
    if root == p || root == q {
        return root
    }
    // Divide
    left := lowestCommonAncestor(root.Left, p, q)
    right := lowestCommonAncestor(root.Right, p, q)


    // Conquer
    // 左右两边都不为空，则根节点为祖先
    if left != nil && right != nil {
        return root
    }
    if left != nil {
        return left
    }
    if right != nil {
        return right
    }
    return nil
}
```

### BFS 层次应用

#### binary-tree-level-order-traversal

[binary-tree-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

> 给你一个二叉树，请你返回其按  **层序遍历**  得到的节点值。 （即逐层地，从左到右访问所有节点）

思路：用一个队列记录一层的元素，然后扫描这一层元素添加下一层元素到队列（一个数进去出来一次，所以复杂度 O(logN)）

```go
func levelOrder(root *TreeNode) [][]int {
	result := make([][]int, 0)
	if root == nil {
		return result
	}
	queue := make([]*TreeNode, 0)
	queue = append(queue, root)
	for len(queue) > 0 {
		list := make([]int, 0)
        // 为什么要取length？
        // 记录当前层有多少元素（遍历当前层，再添加下一层）
		l := len(queue)
		for i := 0; i < l; i++ {
            // 出队列
			level := queue[0]
			queue = queue[1:]
			list = append(list, level.Val)
			if level.Left != nil {
				queue = append(queue, level.Left)
			}
			if level.Right != nil {
				queue = append(queue, level.Right)
			}
		}
		result = append(result, list)
	}
	return result
}
```

#### binary-tree-level-order-traversal-ii

[binary-tree-level-order-traversal-ii](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

> 给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

思路：在层级遍历的基础上，翻转一下结果即可

```go
func levelOrderBottom(root *TreeNode) [][]int {
    result := levelOrder(root)
    // 翻转结果
    reverse(result)
    return result
}
func reverse(nums [][]int) {
	for i, j := 0, len(nums)-1; i < j; i, j = i+1, j-1 {
		nums[i], nums[j] = nums[j], nums[i]
	}
}
func levelOrder(root *TreeNode) [][]int {
	result := make([][]int, 0)
	if root == nil {
		return result
	}
	queue := make([]*TreeNode, 0)
	queue = append(queue, root)
	for len(queue) > 0 {
		list := make([]int, 0)
        // 为什么要取length？
        // 记录当前层有多少元素（遍历当前层，再添加下一层）
		l := len(queue)
		for i := 0; i < l; i++ {
            // 出队列
			level := queue[0]
			queue = queue[1:]
			list = append(list, level.Val)
			if level.Left != nil {
				queue = append(queue, level.Left)
			}
			if level.Right != nil {
				queue = append(queue, level.Right)
			}
		}
		result = append(result, list)
	}
	return result
}
```

#### binary-tree-zigzag-level-order-traversal

[binary-tree-zigzag-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

> 给定一个二叉树，返回其节点值的锯齿形层次遍历。Z 字形遍历

```go
func zigzagLevelOrder(root *TreeNode) [][]int {
	result := make([][]int, 0)
	if root == nil {
		return result
	}
	queue := make([]*TreeNode, 0)
	queue = append(queue, root)
	toggle := false
	for len(queue) > 0 {
		list := make([]int, 0)
		// 记录当前层有多少元素（遍历当前层，再添加下一层）
		l := len(queue)
		for i := 0; i < l; i++ {
			// 出队列
			level := queue[0]
			queue = queue[1:]
			list = append(list, level.Val)
			if level.Left != nil {
				queue = append(queue, level.Left)
			}
			if level.Right != nil {
				queue = append(queue, level.Right)
			}
		}
		if toggle {
			reverse(list)
		}
		result = append(result, list)
		toggle = !toggle
	}
	return result
}
func reverse(nums []int) {
	for i := 0; i < len(nums)/2; i++ {
		nums[i], nums[len(nums)-1-i] = nums[len(nums)-1-i], nums[i]
	}
}
```

### 二叉搜索树应用

#### validate-binary-search-tree

[validate-binary-search-tree](https://leetcode-cn.com/problems/validate-binary-search-tree/)

> 给定一个二叉树，判断其是否是一个有效的二叉搜索树。

思路 1：中序遍历，检查结果列表是否已经有序

思路 2：分治法，判断左 MAX < 根 < 右 MIN

```go
// v1
func isValidBST(root *TreeNode) bool {
    result := make([]int, 0)
    inOrder(root, &result)
    // check order
    for i := 0; i < len(result) - 1; i++{
        if result[i] >= result[i+1] {
            return false
        }
    }
    return true
}

func inOrder(root *TreeNode, result *[]int)  {
    if root == nil{
        return
    }
    inOrder(root.Left, result)
    *result = append(*result, root.Val)
    inOrder(root.Right, result)
}

```

```go
// v2分治法
type ResultType struct {
	IsValid bool
    // 记录左右两边最大最小值，和根节点进行比较
	Max     *TreeNode
	Min     *TreeNode
}

func isValidBST2(root *TreeNode) bool {
	result := helper(root)
	return result.IsValid
}
func helper(root *TreeNode) ResultType {
	result := ResultType{}
	// check
	if root == nil {
		result.IsValid = true
		return result
	}

	left := helper(root.Left)
	right := helper(root.Right)

	if !left.IsValid || !right.IsValid {
		result.IsValid = false
		return result
	}
	if left.Max != nil && left.Max.Val >= root.Val {
		result.IsValid = false
		return result
	}
	if right.Min != nil && right.Min.Val <= root.Val {
		result.IsValid = false
		return result
	}

	result.IsValid = true
    // 如果左边还有更小的3，就用更小的节点，不用4
    //  5
    // / \
    // 1   4
    //      / \
    //     3   6
	result.Min = root
	if left.Min != nil {
		result.Min = left.Min
	}
	result.Max = root
	if right.Max != nil {
		result.Max = right.Max
	}
	return result
}
```

#### insert-into-a-binary-search-tree

[insert-into-a-binary-search-tree](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

> 给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。

思路：找到最后一个叶子节点满足插入条件即可

```go
// DFS查找插入位置
func insertIntoBST(root *TreeNode, val int) *TreeNode {
    if root == nil {
        root = &TreeNode{Val: val}
        return root
    }
    if root.Val > val {
        root.Left = insertIntoBST(root.Left, val)
    } else {
        root.Right = insertIntoBST(root.Right, val)
    }
    return root
}
```

## 总结

* 掌握二叉树递归与非递归遍历
* 理解 DFS 前序遍历与分治法
* 理解 BFS 层次遍历

## 练习

* [ ] [maximum-depth-of-binary-tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
* [ ] [balanced-binary-tree](https://leetcode-cn.com/problems/balanced-binary-tree/)
* [ ] [binary-tree-maximum-path-sum](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)
* [ ] [lowest-common-ancestor-of-a-binary-tree](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
* [ ] [binary-tree-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
* [ ] [binary-tree-level-order-traversal-ii](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)
* [ ] [binary-tree-zigzag-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)
* [ ] [validate-binary-search-tree](https://leetcode-cn.com/problems/validate-binary-search-tree/)
* [ ] [insert-into-a-binary-search-tree](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)
