> Given preorder and inorder traversal of a tree, construct the binary tree.
#### Example：
> preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
output [3,9,20,null,null,15,7]

#### 解释下题目：
> 给定先序遍历和中序遍历的数组，构造出对应一棵树。


## 1. 迭代
#### 实际耗时：8ms
```
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return recursive(preorder, 0, preorder.length, inorder, 0, inorder.length);
    }

    public TreeNode recursive(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd) {
        if (preStart >= preorder.length || inStart >= inorder.length || preStart > preEnd || inStart > inEnd) {
            return null;
        }
        TreeNode root = new TreeNode(preorder[preStart]);

        //找到分割点的下标
        int index = -1;
        for (int i = inStart; i <= inEnd; i++) {
            if (inorder[i] == preorder[preStart]) {
                index = i;
                break;
            }
        }

        root.left = recursive(preorder, preStart + 1, preorder.length, inorder, inStart, index - 1);
        root.right = recursive(preorder, preStart + index - inStart + 1, preorder.length, inorder, index + 1, inEnd);
        return root;

    }

```
&emsp;&emsp;思路：因为是树的问题，所以肯定是递归。然后在先序数组和中序数组中找一个相同的元素（题目中说了所有的节点的值都互不相同），这个元素必定是当前所在的这一层的根元素，然后`inorder`的左边就是左子树，而右边是右子树，递归即可。
###### 时间复杂度O(n^2)
###### 空间复杂度O(1)
---------
