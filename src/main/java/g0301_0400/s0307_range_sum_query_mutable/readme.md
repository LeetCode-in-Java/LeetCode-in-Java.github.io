[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 307\. Range Sum Query - Mutable

Medium

Given an integer array `nums`, handle multiple queries of the following types:

1.  **Update** the value of an element in `nums`.
2.  Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

Implement the `NumArray` class:

*   `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
*   `void update(int index, int val)` **Updates** the value of `nums[index]` to be `val`.
*   `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).

**Example 1:**

**Input**

    ["NumArray", "sumRange", "update", "sumRange"]
    [[[1, 3, 5]], [0, 2], [1, 2], [0, 2]]

**Output:** [null, 9, null, 8]

**Explanation:**

    NumArray numArray = new NumArray([1, 3, 5]);
    numArray.sumRange(0, 2); // return 1 + 3 + 5 = 9
    numArray.update(1, 2); // nums = [1, 2, 5]
    numArray.sumRange(0, 2); // return 1 + 2 + 5 = 8 

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   `-100 <= nums[i] <= 100`
*   `0 <= index < nums.length`
*   `-100 <= val <= 100`
*   `0 <= left <= right < nums.length`
*   At most <code>3 * 10<sup>4</sup></code> calls will be made to `update` and `sumRange`.

## Solution

```java
public class NumArray {
    private int[] tree;
    private int[] nums;

    public NumArray(int[] nums) {
        tree = new int[nums.length + 1];
        this.nums = nums;
        // copy the array into the tree
        System.arraycopy(nums, 0, tree, 1, nums.length);
        for (int i = 1; i < tree.length; i++) {
            int parent = i + (i & -i);
            if (parent < tree.length) {
                tree[parent] += tree[i];
            }
        }
    }

    public void update(int index, int val) {
        int currValue = nums[index];
        nums[index] = val;
        index++;
        while (index < tree.length) {
            tree[index] = tree[index] - currValue + val;
            index = index + (index & -index);
        }
    }

    private int sum(int i) {
        int sum = 0;
        while (i > 0) {
            sum += tree[i];
            i -= (i & -i);
        }
        return sum;
    }

    public int sumRange(int left, int right) {
        return sum(right + 1) - sum(left);
    }
}

/*
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(index,val);
 * int param_2 = obj.sumRange(left,right);
 */
```