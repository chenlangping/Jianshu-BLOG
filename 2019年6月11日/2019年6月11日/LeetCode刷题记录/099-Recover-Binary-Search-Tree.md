> Two elements of a binary search tree (BST) are swapped by mistake.
Recover the tree without changing its structure.
#### Example：
> Input: [1,3,null,null,2]
Output: [3,1,null,null,2]

>Input: [3,1,4,null,null,2]
Output: [2,1,4,null,null,3]

#### Note：
> A solution using O(n) space is pretty straight forward.
Could you devise a constant space solution?

#### 解释下题目：
> 二叉搜索树里有两个节点被交换了，在保持树的结构下把这两个值换回来


## 1. 遍历找
#### 实际耗时：26ms
```
    private TreeNode firstElement = null;
    private TreeNode secondElement = null;
    private TreeNode prevElement = new TreeNode(Integer.MIN_VALUE);

    public void recoverTree(TreeNode root) {
        traverse(root);
        int temp = firstElement.val;
        firstElement.val = secondElement.val;
        secondElement.val = temp;
    }

    private void traverse(TreeNode root) {

        if (root == null)
            return;

        traverse(root.left);

        //接下来是遍历中“应该打印的地方”

        if (firstElement == null && prevElement.val >= root.val) {
            firstElement = prevElement;
        }

        if (firstElement != null && prevElement.val >= root.val) {
            secondElement = root;
        }
        prevElement = root;

        //“打印结束”

        traverse(root.right);
    }
```
###### 踩过的坑：只有两个数被交换了！！我一开始没看到！！！导致把问题极度复杂化了！！
&emsp;&emsp;思路：就是利用遍历的方法找到这两个违反二叉搜索树的数，然后交换就行了。
###### 时间复杂度O(n) n为节点数
###### 空间复杂度O(1)
---------
