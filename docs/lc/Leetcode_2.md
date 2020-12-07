**【原题链接】**

[2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

**【题目描述】**

给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例：**

```text
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

**【代码】**

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        auto dummy = new ListNode(), cur = dummy;
        int t = 0;
        while(l1 || l2 || t){
            if(l1) t += l1->val, l1 = l1->next;
            if(l2) t += l2->val, l2 = l2->next;
            cur = cur->next = new ListNode(t % 10);
            t /= 10;
        }
        return dummy->next;
    }
};
```

> 执行用时: **56 ms**

> 内存消耗: **70.1 MB**

**【时间复杂度】**

O(n) 

**【注意点】**

1. 其他解法：无

1. 需要返回头结点，所以定义虚拟头结点dummy

1. 注意当前节点cur一定要更新到cur->next

1. new ListNode()可以不给初值

