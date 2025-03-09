[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 373\. Find K Pairs with Smallest Sums

Medium

You are given two integer arrays `nums1` and `nums2` sorted in **ascending order** and an integer `k`.

Define a pair `(u, v)` which consists of one element from the first array and one element from the second array.

Return _the_ `k` _pairs_ <code>(u<sub>1</sub>, v<sub>1</sub>), (u<sub>2</sub>, v<sub>2</sub>), ..., (u<sub>k</sub>, v<sub>k</sub>)</code> _with the smallest sums_.

**Example 1:**

**Input:** nums1 = [1,7,11], nums2 = [2,4,6], k = 3

**Output:** [[1,2],[1,4],[1,6]]

**Explanation:** The first 3 pairs are returned from the sequence: [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]

**Example 2:**

**Input:** nums1 = [1,1,2], nums2 = [1,2,3], k = 2

**Output:** [[1,1],[1,1]]

**Explanation:** The first 2 pairs are returned from the sequence: [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]

**Example 3:**

**Input:** nums1 = [1,2], nums2 = [3], k = 3

**Output:** [[1,3],[2,3]]

**Explanation:** All possible pairs are returned from the sequence: [1,3],[2,3]

**Constraints:**

*   <code>1 <= nums1.length, nums2.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums1[i], nums2[i] <= 10<sup>9</sup></code>
*   `nums1` and `nums2` both are sorted in **ascending order**.
*   `1 <= k <= 1000`

## Solution

```java
import java.util.AbstractList;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        return new AbstractList<List<Integer>>() {
            private List<List<Integer>> pairs;

            @Override
            public List<Integer> get(int index) {
                init();
                return pairs.get(index);
            }

            @Override
            public int size() {
                init();
                return pairs.size();
            }

            private void load() {
                int n = nums1.length;
                int m = nums2.length;
                int left = nums1[0] + nums2[0];
                int right = nums1[n - 1] + nums2[m - 1];
                int middle;

                while (left <= right) {
                    middle = (left + right) / 2;
                    long count = getCount(nums1, nums2, middle, k);
                    if (count < k) {
                        left = middle + 1;
                    } else if (count > k) {
                        right = middle - 1;
                    } else {
                        left = middle;
                        break;
                    }
                }
                getPairs(nums1, nums2, left, k);
            }

            private long getCount(int[] nums1, int[] nums2, int goal, int k) {
                int prevRight = nums2.length - 1;
                int count = 0;

                for (int i = 0; i < nums1.length && nums1[i] + nums2[0] <= goal; i++) {
                    int left = 0;
                    int right = prevRight;
                    int position = -1;
                    while (left <= right) {
                        int middle = (right + left) / 2;
                        int sum = nums1[i] + nums2[middle];
                        if (sum <= goal) {
                            position = middle;
                            left = middle + 1;
                        } else {
                            right = middle - 1;
                        }
                    }
                    if (position >= 0) {
                        count += position + 1;
                        prevRight = position;
                    }
                    if (count > k) {
                        return count;
                    }
                }
                return count;
            }

            private void getPairs(int[] nums1, int[] nums2, int sum, int k) {
                pairs = new ArrayList<>();
                for (int item : nums1) {
                    for (int j = 0; j < nums2.length && item + nums2[j] < sum; j++) {
                        pairs.add(Arrays.asList(item, nums2[j]));
                    }
                }
                for (int value : nums1) {
                    for (int j = 0;
                            j < nums2.length && value + nums2[j] <= sum && pairs.size() < k;
                            j++) {
                        if (value + nums2[j] == sum) {
                            pairs.add(Arrays.asList(value, nums2[j]));
                        }
                    }
                }
            }

            public void init() {
                if (null == pairs) {
                    load();
                }
            }
        };
    }
}
```