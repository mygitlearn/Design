### Sort a linked list in O(n log n) time using constant space complexity.（使用常数空间复杂度对O（n log n）时间中的链表排序。）
#### 归并排序
#### 因为题目要求复杂度为O(nlogn),故可以考虑归并排序的思想。
- 归并排序的一般步骤为：
- 1）将待排序数组（链表）取中点并一分为二；
- 2）递归地对左半部分进行归并排序；
- 3）递归地对右半部分进行归并排序；
- 4）将两个半部分进行合并（merge）,得到结果。

<pre>C语言版
class Solution {
public:
    ListNode *sortList(ListNode *head) {

    if (!head || !head->next) return head;
        // 找到中间节点, 定位到中间节点的前驱节点
        ListNode* fast = head, * slow = head;
        while (fast->next && fast->next->next) {
            fast = fast->next->next;
            slow = slow->next;
        }
        ListNode* mid = slow->next; // 中间节点
        slow->next = nullptr;   // 断开链表
        // 递归地对左右半部分进行归并排序
        ListNode* list1 = sortList(head);
        ListNode* list2 = sortList(mid);
        // 合并左右部分的有序链表
        ListNode* sortedList = mergeList(list1, list2);
        return sortedList;  
    }

    ListNode* mergeList(ListNode* list1, ListNode* list2) {
        if (list1 == nullptr) return list2;
        if (list2 == nullptr) return list1;
 
        ListNode* head = new ListNode(-1);
        ListNode* tmp = head;
 
        while (list1 && list2) {
            if (list1->val < list2->val) {
                tmp->next = list1;
                list1 = list1->next;
            } else {
                tmp->next = list2;
                list2 = list2->next;
            }
            tmp = tmp->next;
        }
        if (list1) tmp->next = list1;
        if (list2) tmp->next = list2;
        return head->next;
    }
};

</pre>

<pre>Java版,耗时856ms，占用内存37400k
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */

public class Solution {
    public ListNode sortList(ListNode head) {
        quickSort(head, null);
        return head;
    }
    public static void quickSort(ListNode head, ListNode end) {
        if(head != end) {
            ListNode partion = partion(head);
            quickSort(head, partion);
            quickSort(partion.next, end);
        }
    }

    public static ListNode partion(ListNode head) {
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null) {
            if(fast.val < head.val) {
                slow = slow.next;
                fast.val = slow.val ^ fast.val ^ (slow.val = fast.val);
            }
            fast = fast.next;
        }
        slow.val = head.val ^ slow.val ^ (head.val = slow.val);
        return slow;
    }
}
</pre>