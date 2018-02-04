Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.
```
Example 1:
Input: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
Output: 
Merged tree:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
Note: The merging process must start from the root nodes of both trees.
```
Test Case1:
[1,3,2,5]
[2,1,3,null,4,null,7]

***
2018-02-04
1. My Solution:递归遍历二叉树合并。
183 / 183 test cases passed.
Status: Accepted
Runtime: 13 ms
Your runtime beats 87.91 % of java submissions.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null)
        {
            return t2 == null ? null : t2;
        }
        //这里注意，最开始没写这个分支，导致示例testcase都没通过。。
        if(t2 == null)
        {
            return t1 == null ? null : t1;
        }
        TreeNode merged = new TreeNode(t1.val + t2.val);
        merged.left = mergeTrees(t1.left, t2.left);
        merged.right = mergeTrees(t1.right, t2.right);
        return merged;
    }
}
```


2. 搬运讨论区其他人的类似解法，写法更简洁，我原封不动的提交了一次，耗时13ms。看来是LeetCode的服务器性能有改善。。
My Java 16ms solution:
```java
public class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null) return t2;
        if (t2 == null) return t1;
        TreeNode result = new TreeNode(t1.val + t2.val);
        result.left = mergeTrees(t1.left, t2.left);
        result.right = mergeTrees(t1.right, t2.right);
        return result;
    }
}
```
I think in my way the checking conditions are much easier to understand. But the ideas are same.


3. 还有人用循环，深度优先和广度优先分别遍历树，然后合并。比较复杂，暂时还没耐心看完，先贴在这：
https://leetcode.com/problems/merge-two-binary-trees/discuss/104331/Java-One-Recursive-Solution-and-Two-Iterative-Solutions-(DFS-and-BFS)-with-Explanations

代码稍微简洁点的DFS：
https://leetcode.com/problems/merge-two-binary-trees/discuss/104403/DFS-solutionsame-idea-with-%22Q100.-Same-Tree%22
