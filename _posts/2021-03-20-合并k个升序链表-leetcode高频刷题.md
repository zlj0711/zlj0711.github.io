---
layout:     post
title:      合并k个升序链表
subtitle:   leetcode高频刷题
date:       2021-03-20
author:     zlj
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - leetcode
    - leetcode高频刷题
	- 链表
---



其实和之前做过的三数之和差不多，但是就是简化的那块总是出问题，不想改了。

自己写的这个合并算法，小数据可以跑，大数据会超时

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* head1, ListNode* head2){
        ListNode* ans = new ListNode(0);
        ListNode* tail = ans;
        while(head1 != nullptr && head2 != nullptr){
            if(head1->val < head2->val){
                ListNode* tmp = new ListNode(head1->val);
                tail->next = tmp;
                tail = tmp;
                head1 = head1->next;
            }
            else{
                ListNode* tmp = new ListNode(head2->val);
                tail->next = tmp;
                tail = tmp;
                head2 = head2->next;
            }
        }
        while(head1 != nullptr){
            ListNode* tmp = new ListNode(head1->val);
            tail->next = tmp;
            tail = tmp;
            head1 = head1->next;
        }
        while(head2 != nullptr){
            ListNode* tmp = new ListNode(head2->val);
            tail->next = tmp;
            tail = tmp;
            head2 = head2->next;
        }
        return ans->next;
    }
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.size() < 1){
            return nullptr;
        }
        if(lists.size() == 1){
            return lists[0];
        }
        int i = 0;
        while(i < lists.size() - 1){
            lists[i + 1] = mergeTwoLists(lists[i], lists[i + 1]);
            i ++;
        }
        return lists[lists.size() - 1];
    }
};
```

