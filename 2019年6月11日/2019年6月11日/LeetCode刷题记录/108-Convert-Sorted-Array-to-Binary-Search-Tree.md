>Given an array where elements are sorted in ascending order, convert it to a height balanced BST.
For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.
#### Example：
> input：[-10,-3,0,5,9]
output：[0,-10,5,null,-3,null,9]

#### 解释下题目：
> 给定一个升序的数组，构造一个平衡的二叉搜索树


## 1. 二分法
#### 实际耗时：0ms
```
    public TreeNode sortedArrayToBST(int[] nums) {
        return recursive(nums, 0, nums.length - 1);
    }

    public TreeNode recursive(int[] nums, int low, int high) {
        if (low > high) {
            return null;
        }
        int mid = (low + high) >> 1;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = recursive(nums, low, mid - 1);
        root.right = recursive(nums, mid + 1, high);
        return root;
    }
```
&emsp;&emsp;思路就是二分法，没什么好说的
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
