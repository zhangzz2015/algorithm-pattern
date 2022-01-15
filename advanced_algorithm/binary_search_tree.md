# 二叉搜索树

## 定义

* 每个节点中的值必须大于（或等于）存储在其左侧子树中的任何值。
* 每个节点中的值必须小于（或等于）存储在其右子树中的任何值。

## 应用

[validate-binary-search-tree](https://leetcode.com/problems/validate-binary-search-tree/)

> 验证二叉搜索树

思路：方法1：考虑二叉搜索树特点，可使用中序遍历，记录前一个节点，判断是否递增。方法2：可使用记录最大和最小的node来判断。

```go
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        
        return dfs(root, NULL, NULL); 
    }
    
    bool dfs(TreeNode* root, TreeNode*  minnode, TreeNode* maxnode)
    {
        if(root==NULL)
            return true; 
        
        if( (minnode && minnode->val >=root->val ) || (maxnode && root->val >=maxnode->val) )
            return false; 
        
        if(dfs(root->left, minnode, root) ==false)
           return false; 
        
        if(dfs(root->right,root, maxnode) == false)
           return false; 
           
        return true;                                
    }
};
```

[insert-into-a-binary-search-tree](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

> 给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 保证原始二叉搜索树中不存在新值。

```go
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        
        if(root==NULL)
        {
            TreeNode* current = new TreeNode( val); 
            return current; ; 
        }
        
        if(root->val > val)
            root->left = insertIntoBST(root->left, val);
        else
            root->right = insertIntoBST(root->right, val); 
        
        return root; 
        
    }
};
```

[delete-node-in-a-bst](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

> 给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的  key  对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

```go
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        
        
        return dfs(root, key); 
    }
    
    
    TreeNode* dfs(TreeNode* root, int key)
    {
        if(root== NULL)
            return NULL; 
        
        if(root->val == key) // remove
        {
            if(root->left==NULL)
            {
                TreeNode* ret = root->right;                 
                delete root;                 
                return ret;
            }
            if(root->right==NULL)
            {
                TreeNode* ret = root->left; 
                delete root; 
                return ret; 
            }
            ///  both has left and right. 
            // find root->right most left value. 
            TreeNode* most_left = NULL; 
            TreeNode* current =root->right; 
            while(current)
            {
                most_left = current; 
                current = current->left; 
            } 
            root->val = most_left->val; 
            root->right = dfs(root->right, root->val);             
            return root; 
            
        }
        else if(root->val>key)
        {
          root->left=  dfs(root->left, key); 
        }
        else
        {
          root->right =  dfs(root->right, key); 
        }
        
        return root; 
    }
};
```

[balanced-binary-tree](https://leetcode-cn.com/problems/balanced-binary-tree/)

> 给定一个二叉树，判断它是否是高度平衡的二叉树。

```go
type ResultType struct{
    height int
    valid bool
}
func isBalanced(root *TreeNode) bool {
    return dfs(root).valid
}
func dfs(root *TreeNode)(result ResultType){
    if root==nil{
        result.valid=true
        result.height=0
        return
    }
    left:=dfs(root.Left)
    right:=dfs(root.Right)
    // 满足所有特点：二叉搜索树&&平衡
    if left.valid&&right.valid&&abs(left.height,right.height)<=1{
        result.valid=true
    }
    result.height=Max(left.height,right.height)+1
    return
}
func abs(a,b int)int{
    if a>b{
        return a-b
    }
    return b-a
}
func Max(a,b int)int{
    if a>b{
        return a
    }
    return b
}
```

## 练习

* [ ] [validate-binary-search-tree](https://leetcode-cn.com/problems/validate-binary-search-tree/)
* [ ] [insert-into-a-binary-search-tree](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)
* [ ] [delete-node-in-a-bst](https://leetcode-cn.com/problems/delete-node-in-a-bst/)
* [ ] [balanced-binary-tree](https://leetcode-cn.com/problems/balanced-binary-tree/)
