---
layout:     post
title:      删除链表的倒数第n个节点
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



常见的双指针方法，注意一些特殊情况就可以

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(head->next == nullptr){
            return nullptr;
        }
        ListNode* a = head;
        ListNode* b = head;
        ListNode* pre = new ListNode(0);
        pre->next = head;
        if(n > 1){
            for(int i = 1;i < n;i ++){
                b = b->next;
            }
        }       
        while(b->next != nullptr){
            b = b->next;
            a = a->next;
            pre = pre->next;
        }
        pre->next = a->next;
        if(a == head){
            return a->next;
        }
        return head;
    }
};
```

