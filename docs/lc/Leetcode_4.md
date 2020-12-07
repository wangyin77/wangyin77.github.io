**【原题链接】**

[4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

**【题目描述】**

给定两个大小为 m 和 n 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的中位数。

**进阶：**你能设计一个时间复杂度为 `O(log (m+n))` 的算法解决此问题吗？

**示例 1：**

```text
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

**示例 2：**

```text
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

**示例 3：**

```text
输入：nums1 = [0,0], nums2 = [0,0]
输出：0.00000
```

**示例 4：**

```text
输入：nums1 = [], nums2 = [1]
输出：1.00000
```

**示例 5：**

```text
输入：nums1 = [2], nums2 = []
输出：2.00000
```

**提示：**

- `nums1.length == m`

- `nums2.length == n`

- `0 <= m <= 1000`

- `0 <= n <= 1000`

- `1 <= m + n <= 2000`

- `-106 <= nums1[i], nums2[i] <= 106`

**【代码】**

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int k = nums1.size() + nums2.size();
        if(k % 2 == 0){
            int left = find(nums1, 0, nums2, 0, k / 2);
            int right = find(nums1, 0, nums2, 0, k / 2 + 1);
            return (left + right) / 2.0;
        }else
            return find(nums1, 0, nums2, 0, k / 2 + 1); // k / 2 + 1,要记得加1
    }
    int find(vector<int>& nums1, int i, vector<int>& nums2, int j , int k){
        if(nums1.size() - i > nums2.size() - j) return find(nums2, j, nums1, i, k);
        if(nums1.size() == i) return nums2[j + k - 1];
        if(k == 1)  return min(nums1[i], nums2[j]);
        int si = min((int)nums1.size(), i + k / 2), sj = j + k - k / 2;
        //nums1比较短，可能会没有k/2个数，就拿尾节点和nums2的k/2比较就可以
        if(nums1[si - 1] > nums2[sj - 1])//si,sj都是指向要比较的点后一个的位置
            return find(nums1, i, nums2, sj, k - (sj - j));
        else
            return find(nums1, si, nums2, j, k - (si - i));
    }
};
```

> 执行用时: **40 ms**

> 内存消耗: **87.4 MB**

**【时间复杂度】**

O(log(n+m)) 

**【注意点】**

1. 其他解法：合并后排序(O((n+m)log(n+m)));二分O(log(min(n,m)))切分非常复杂。

1. 需要返回double类型的数，所以要除2.0

1. **find函数的思想：找nums1,nums2中的第k小数，可以比较nums1[k/2]和nums2[k/2]，小的那个前面的k/2个数就可以直接舍弃，遍历小的那个列k/2之后的数和大的那个列，的k'小数，直到一个列遍历玩，或者k=1，结束递归。**

